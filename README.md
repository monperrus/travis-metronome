# Travis Metronome

`Travis Metronome` is a piece of software art symbolizing perpetual motion in the digital world. It consists of two units of computation (the "tick" and the "tock") that are forever alternating: the tick triggers the tock, the tock triggers the tick, et voila. Ticks and tocks do a minimalist task: enumerating 42 times on the console the sequence of numbers from [1 to 42](https://travis-ci.com/github/monperrus/travis-metronome/builds/196880461), creating an infinite regress in the meaning of digital life. What to do if 42 is not an option? There is only one solution: try harder.

![Screenshot from 2020-10-31 05-49-29](https://user-images.githubusercontent.com/803666/97771434-e0480e00-1b34-11eb-9e90-82411cd4bcac.png)

You can watch the live installation of `Travis Metronome` at <https://travis-ci.com/github/monperrus/travis-metronome>. The tick-tock alternation happens every 29 minutes (42*42 seconds), so there is always either one [tick](https://travis-ci.com/github/monperrus/travis-metronome) or one [tock](https://travis-ci.com/github/monperrus/travis-metronome-tok) running.

Author: Martin Monperrus  
October 2020

## Implementation

The [tick](https://github.com/monperrus/travis-metronome) and the [tock](https://github.com/monperrus/travis-metronome-tok) are two Github repositories; each one configured with continuous integration provided by the great provider [Travis CI](https://travis-ci.com/). The implementation is in a single file, the `.travis.yml` configuration file, in which two bash commands are inserted. The tick triggers the tock via the [Travis API](https://docs.travis-ci.com/user/triggering-builds/) (and vice versa).

```shell
language: minimal

script:
  - for i in `seq 42`; do sleep 42; echo $i; done
  - curl -X POST -H Accept\:\ application/json -H Travis-API-Version\:\ 3 -H Content-Type\:\ application/json -H Authorization\:\ token\ $TRAVIS_TOKEN https://api.travis-ci.com/repo/monperrus\%2Ftravis-metronome-tok/requests
```

The list of tick builds is at <https://travis-ci.com/github/monperrus/travis-metronome/builds/> and there is the mirror [list of tock builds](https://travis-ci.com/github/monperrus/travis-metronome-tok/builds/).

For every commit to the tick or tock repository, there is a triggered build, which comes on top of the previous tick-tock build chain. Having those multiple tick-tocks is inelegant. So, in order to have one single tick-tock, I manually cancel the new build for a new commit, in order to keep a single build chain.

## Verification of the build chain

Every time a build is created, the newly created build identifier is output on the console, in the CI log. This means that one can measure and verify the length of the build chain by taking the first tik build, extracting the next tock build identifier, reading its log, extracting the next tick build identifier, and loop forever. The code for doing this verification is available at <https://github.com/monperrus/travis-metronome/issues/1>.

## Credits

This piece of sotware art is a follow-up on the concept of [continuous integration art](https://www.youtube.com/watch?v=XUZj1eXal4s) which we created last year at KTH Royal Institute of Technology. It is founded on a personal ethics of [software minimalism](https://en.wikipedia.org/wiki/Minimalism_(computing)), with deep insipration from the Demoscene people who create programs that are [256 bytes or less in size](http://www.sizecoding.org/). The energy of the [Pellow week](https://rethread.art/projects/pellow.html) certainly diffuses at the time of creating this. I thank Nicolas Harrand for the name `Travis Metronome` and Travis CI for donating the computing power to run the installation.






# dockerize

*modulus docker talk notes*

##### dockerfile

* single responsibility
* use backslash \ the separate lines
* pin down versions (docker v162, lock down so don't get stung when upgrade to v167)
* install and cleanup need to be in the same layer
* break out sections (php/imagemagick) that will be updated frequently, add a cheat line to force a rebuild


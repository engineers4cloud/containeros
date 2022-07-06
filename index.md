### What is containerOS ?
A very slim container-OS based on Toybox

containerOS is based on the great work of the developers of [toybox](https://github.com/landley/toybox) with the utmost respect.

containerOS has no package managers and nothing that is really not needed to run.

containerOS is stripped down to the bare essentials needed to run a container.

### Image Derivates
#### base
The base image comes with the following set of statically linked binaries: sh, ls, cat, ps, id, env, chown, chmod, cp, date, mv, mkdir. 

#### slim
The slim image is slightly smaller than the base image and comes with the following set of statically linked binaries: sh, ls, cat, ps, id, env. 

To test rocm57 ....

1.  make sure linux is rocm docker ready  (see AMD rocm doc,  wsl or any sort of Linux VM will NOT work!)
2.  `cd rocm57`  then  `sh ./buildimage.sh`, this will create the `mlcllmrocm57:v0.1` base image; you can do a `docker images` to verify
3.  from top level,  `cd test` and see that the `cache` directory exists and that you have write permissions, this is where all the weights will be downloaded the first time, and will be managed for you
4.  run `sh startrocm57chat.sh gemma-2b-it-q4f16_1`
5.  this should download and convert all the weights (the first time you run it) and then start a chat session with the extra-small gemma model (on your rocm supported GPU)
6.  try the prompt `list the states in the usa` to get a somewhat stable reply 

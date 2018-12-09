# Imgur Uploader #

This repository is an asynchronous file uploader to Imgur which operates in-memory, written in Python using Sanic framework.


### Setup ###

#####  Run the Dockerfile in the root folder

* Create docker image <br />
`docker build -t imgur .`

* Create docker container from imgur image <br />
`docker run -p -d 80:8001 --name imgur_app imgur`

* In case of rerun <br />
`docker start imgur_app`


##### How to run tests
* Run the following command from the root folder <br />
`pytest`

### APIs

APIs can be tested over any REST client with base url = 0.0.0.0

* POST - `/v1/images/upload` - URLs to be uploaded.
* GET - `/v1/images` - Get uploaded images.
* GET - `/v1/images/upload/<jobId>` - Get status of job.

#### Assumptions
* The docker ports would be mapped according to the documentation.
* Redis is considered as external database(snapshots in external files), and for the purpose of strictly sticking to in-memory
 data structures - Celery and Redis have been avoided. 
* Asychronous operations achieved using coroutines and semaphores. 
* Lack of external reliable data storage, results in loss of status in case of server restart, and is limited by RAM allocations. 
* For invalid jobId in `/v1/images/upload/<jobId>` server throws Internal Server Error for simplicity.
* log file under log directory.  

##### Known issues
* Sanic is an asynchrounous python framework, and is relatively new, and hence for the given tasks an issue was encountered in testing, 
pytest-sanic library doesn't support testing multiple request under the same test case and the application for it's thorough testing needs such a test case.
This is a known issue, see [this](https://github.com/huge-success/sanic/issues/988), and requires more time is exploring options around the problem.
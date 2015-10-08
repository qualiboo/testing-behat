# Behat with Docker

This image provides a way to run Behat easily with Docker. 

## Usage

Initialize environment:
    
    docker pull qualiboo/testing-behat    
    mkdir -p behat/build
    docker run -v $(pwd)/behat:/var/work qualiboo/testing-behat --init 

Then run your tests:

    docker run --rm -t -v $(pwd)/behat:/var/work qualiboo/testing-behat

## More

You can easily run your tests with Selenium Grid. 3 browsers are installed (phantomjs, firefox, chrome). 

Create a `docker-compose.yml` file:

    behat:
      image: qualiboo/testing-behat
      volumes:
        - ./behat:/var/work
      links:
          - hub
      environment:
        website: http://blog.lepine.pro
    hub:
      image: qualiboo/testing-hub
      ports:
        - 4444:4444
    firefox:
      image: qualiboo/testing-node-firefox
      ports:
        - 5900
      links:
        - hub
      environment:
        REMOTE_HOST_PARAM: "-maxSession 3 -browser browserName=firefox,maxInstances=3"
    chrome:
      image: qualiboo/testing-node-chrome
      ports:
        - 5900
      links:
        - hub
        
Then run your tests:

    docker-compose run behat --profile=<browser>
    docker-compose run behat --profile=firefox
    docker-compose run behat --profile=chrome
    docker-compose run behat --profile=phantomjs
    
You can also increase the number of available browsers. For example, if you want to have 5 firefox:

    docker-compose scale firefox=5

## Author

Jean-François Lépine <www.qualiboo.com>

## License

See the LICENSE file.
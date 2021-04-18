Stream generator from IMDb files for use in Spark labs. It generates votes
for titles according to the average rank found in title.rankings file
from https://www.imdb.com/interfaces/. Each vote is a single text line with
the title identifier and a numeric score.

Building:

    $ mvn package
    $ docker build -t streamgen .

Usage as a local process, listening on localhost:12345:

    $ java -jar target/streamgen-1.0-SNAPSHOT.jar ./title.ratings.tsv.gz 120
    
Usage as a docker container in a Hadoop network for external access,
listening on localhost:12345:

    $ docker run --env-file hadoop.env --network docker-hadoop_default \
        -p 12345:12345 streamgen hdfs:///input/title.ratings.tsv 120

Usage as a docker container in a Hadoop network for internal access,
listening on streamgen:12345:

    $ docker run --env-file hadoop.env --network docker-hadoop_default \
        --name streamgen streamgen hdfs:///input/title.ratings.tsv 120

It takes two optional parameters: the first is the IMDb title.ratings file
to use. The second is the number of events generated each minute.

To test use nc or telnet as follows:

    $ nc localhost 12345

# cluster-sharding

install mongodb 3.0+

sbt assembly will create 3 jars; seed,backend and rest

bootstrap two seed nodes on ports 8000/8001 (use option -Dakka.remote.netty.tcp.port=8000 etc)

start as many other backend nodes as you like (on unique ports)

start the spray server (uses 8080)

add with POST to /customer/{customer_id}

post body of form:
{
    "name" : "name",
    "data1" : "blah blah",
    "data2" : "whatever"
}

get the list with GET to /customer/{customer_id}

delete with POST to /customer/{customer_id}/{record_uuid}

update with PATCH to /customer/{customer_id}/{record_uuid}
with a body of form :
{
    "name": "name_updated"
}

omit a field in the update to leave it unchanged

customer shard actor state will be passivated after 5 minutes of inactivity.

if the shard dies then once the node crash is detected and removed from the cluster the data will be auto recovered to another node


Event sourced query side:

GET to /customer/{customer_id}/history

will retrieve all historical events for the specified customer


---------------------------------------------------------------------
java -Dakka.remote.netty.tcp.port=8000 -jar ~/code/scala/cluster-sharding/seed/target/scala-2.11/seed-assembly-0.1.0-SNAPSHOT.jar

java -Dakka.remote.netty.tcp.port=8001 -jar ~/code/scala/cluster-sharding/seed/target/scala-2.11/seed-assembly-0.1.0-SNAPSHOT.jar


java -Dakka.remote.netty.tcp.port=9000 -jar ~/code/scala/cluster-sharding/backend/target/scala-2.11/backend-assembly-0.1.0-SNAPSHOT.jar

java -jar ~/code/scala/cluster-sharding/rest/target/scala-2.11/rest-assembly-0.1.0-SNAPSHOT.jar

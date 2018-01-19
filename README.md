# Spark in Practice

## Learn How Spark Work in Simple Example

```shell
# Hive startup commande
spark-shell
```

### Build a RDD from file :

```scala
val Alice= sc.textFile("file:<file path>/Alice.txt")
```

### Count the number of RDD elements :
```scala
	Alice.count()
```
### View the first element of RDD :
```scala
	Alice.first()
```
### Return a new RDD containing only the lines with a specific word :

```scala
  val lignes = Alice.filter(line => line.contains("walking"))
	val sup = lignes.saveAsTextFile("file:/home/cloudera/walking")
	lignes.collect()
```
### Count the number of occurrences of a specific word :
```scala
	val words = Alice.flatMap(line => line.split(" "))
	words.filter(l => l.contains("was")).count()
```
### Return the largest line in the text:
```scala
	val longueursLignes = Alice.map(l => (l.length,l))
	longueursLignes.sortByKey(false,1).first()
```
### Show anagram words list : 
```scala
	val textPure= Alice.map(line => (line.toLowerCase().replace(":"," ").replace("*"," ").replace("/"," ").replace("["," ").replace("]"," ").replace(","," ").replace("\""," ").replace("-"," ").replace("`"," ").replace("'"," ").replace("_"," ").replace(";"," ").replace("."," ").replace("!"," ").replace("?"," ")))
	val words = textPure.flatMap(line => line.split(" "))
	val word = words.distinct()
	val wordl = word.map(line => (line.toList.sorted,line))
	val anag = wordl.reduceByKey((a,b) => if(a.contains(b)) a else a + ", " + b)
	anag.collect
	val w = anag.saveAsTextFile("file:/home/cloudera/anagram3")
  ```

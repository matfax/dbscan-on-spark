# DBSCAN on Spark

[![](https://jitpack.io/v/matfax/dbscan-on-spark.svg)](https://jitpack.io/#matfax/dbscan-on-spark)

### Overview

This is an implementation of the [DBSCAN clustering algorithm](http://en.wikipedia.org/wiki/DBSCAN) 
on top of [Apache Spark](http://spark.apache.org/). It is loosely based on the paper from He, Yaobin, et al.
["MR-DBSCAN: a scalable MapReduce-based DBSCAN algorithm for heavily skewed data"](http://www.researchgate.net/profile/Yaobin_He/publication/260523383_MR-DBSCAN_a_scalable_MapReduce-based_DBSCAN_algorithm_for_heavily_skewed_data/links/0046353a1763ee2bdf000000.pdf). 


I have also created a [visual guide](http://www.irvingc.com/visualizing-dbscan) that explains how the algorithm works.

DBSCAN on Spark is built against Scala 2.11.


### Example usage 


I have created a [sample project](https://github.com/irvingc/dbscan-on-spark-example) 
showing how DBSCAN on Spark can be used. The following however should give you a
good idea of how it should be included in your application.

```scala
import org.apache.spark.mllib.clustering.dbscan.DBSCAN

object DBSCANSample {

  def main(args: Array[String]) {

    val conf = new SparkConf().setAppName("DBSCAN Sample")
    val sc = new SparkContext(conf)

    val data = sc.textFile(src)

    val parsedData = data.map(s => Vectors.dense(s.split(',').map(_.toDouble))).cache()

    log.info(s"EPS: $eps minPoints: $minPoints")

    val model = DBSCAN.train(
      parsedData,
      eps = eps,
      minPoints = minPoints,
      maxPointsPerPartition = maxPointsPerPartition)

    model.labeledPoints.map(p =>  s"${p.x},${p.y},${p.cluster}").saveAsTextFile(dest)

    sc.stop()
  }
}
```

### License

DBSCAN on Spark is available under the Apache 2.0 license. 
See the [LICENSE](LICENSE) file for details.


### Credits

DBSCAN on Spark is maintained by Irving Cordova (irving@irvingc.com).






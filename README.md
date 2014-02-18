clojure-stack-experiment
========================

See how a combination of ClojureScript, Clojure and Datomic work together in a solution.

# Data Model (Datomic-like notation)

* Player
* Character
  * character/name
    * type - string
    * cardinality - one
    * unique - value
    * index - true (due to unique)
  * character/player
    * type - ref (points to Player)
    * cardinality - one
    * component - false (do not want destroy Plaer if Character is destroyed)
  * character/point-total
    * type - long
    * cardinality - one
  * character/height
    * type - long (in centimeters)
    * cardinality - one
  * character/weight
    * type - long (in centigrams)
    * cardinality - one
  * character/age
    * type - long (in days since birth)
    * cardinality - one
  * character/unspent-points
    * type - long
    * cardinality - one
  * character/strength
    * type - long
    * cardinality - one
  * character/dexterify
    * type - long
    * cardinality - one
  * character/intelligence
    * type - long
    * cardinality - one
  * character/health
    * type - long
    * cardinality - one
  * character/hit-points
    * type - long
    * cardinality - one
  * chracter/will
    * type - long
    * cardinality - one
  * character/perception
    * type - long
    * cardinality - one
  * character/fatique-points
    * type - long
    * cardinality - one

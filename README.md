clojure-stack-experiment
========================

See how a combination of ClojureScript, Clojure and Datomic work together in a solution.

# Data Model (Datomic-like notation)

* Player
* Damage Descriptor (static values that can be pre-loaded)
    * damage/strength-attribute (key based on character's strength)
      * type - long
      * cardinality - one
    * damage/thrust-dice-count
      * type - long
      * cardinality - one
    * damage/thrust-modifier
      * type - long
      * cardinality - one
    * damage/swing-dice-count
      * type - long
      * cardinality - one
    * damage/swing-modifier
      * type - long
      * cardinality - one
* Language Descriptor
  * language/name
      * type - string
      * cardinality - one
  * language/spoken-fluency
      * type - long (I really want an enum here: None, Broken, Accented, Native)
      * cardinality - one
  * language/written-fluency
      * type - long (I really want an enum here: None, Broken, Accented, Native)
      * cardinality - one
* Character
  * character/name
    * type - string
    * cardinality - one
    * unique - value
    * index - true (due to unique)
  * character/player
    * type - ref (points to Player)
    * cardinality - one
    * component - false (do not want destroy Player if Character is destroyed)
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
  * character/basic-lift
    * type - long
    * cardinality - one
  * character/basic-speed
    * type - long
    * cardinality - one
  * character/basic-move
    * type - long
    * cardinality - one
  * character/damage-descriptor
    * type - ref (points to Damage Descriptor)
    * cardinality - one
    * component - false (do not want destroy Descriptor if Character is destroyed)
  * character/languages
    * type - ref (points to Language Descriptor)
    * cardinality - many
    * component - true (destroy Descriptor if Character is destroyed)

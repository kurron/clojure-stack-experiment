clojure-stack-experiment
========================

See how a combination of ClojureScript, Clojure and Datomic work together in a solution.

# Data Model (Datomic-like notation)

* Player
* Quirk
  * quirk/name 
    * type - string
    * cardinality - one
  * quirk/type (want enum of Mental or Physical)
    * type - long
    * cardinality - one
* Skill
  * skill/level 
    * type - long
    * cardinality - one
  * quirk/type
    * type - ref (points to a Skill Descriptor)
    * cardinality - one
  * skill/cost (how much was paid to acquire the skill) 
    * type - long
    * cardinality - one
* Skill Default Descriptor
  * skill-default/attribute
    * type - long (want enum of Streth, Dexterity, Intelligence, Health)
    * cardinality - one
  * skill-default/modifier
    * type - long
    * cardinality - one
* Skill Descriptor (static values that can be pre-loaded)
  * skill/name
    * type - string
    * cardinality - one
  * skill/controlling-attribute
    * type - long (want enum of Streth, Dexterity, Intelligence, Health)
    * cardinality - one
  * skill/level
    * type - long (want enum of Easy, Average, Hard)
    * cardinality - one
  * skill/description
    * type - string
    * cardinality - one
  * skill/techology-based
    * type - boolean
    * cardinality - one
  * skill/default
    * type - ref (points to Skill Default Descriptor)
    * cardinality - one
* Trait (Advantage or Disadvantage, static values that can be pre-loaded)
  * trait/name
    * type - string
    * cardinality - one
  * trait/cost
    * type - long
    * cardinality - one
  * trait/modifier
    * type - long
    * cardinality - one
  * trait/description
    * type - string
    * cardinality - one
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
  * character/appearance
    * type - long (want an enum: Hideous, Ugly, Unattractive, Average, Attractive, Beautiful, Very Beautiful)
    * cardinality - one
  * character/status
    * type - long
    * cardinality - one
  * character/reputation
    * type - long
    * cardinality - one
  * character/techology-level (vague, says "Whatever your world's level is" )
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
  * character/traits
    * type - ref (points to Trait)
    * cardinality - many
    * component - false (do not destroy Trait if Character is destroyed)
  * character/quirks
    * type - ref (points to Quirk)
    * cardinality - many
    * component - true (destroy Quirk if Character is destroyed)

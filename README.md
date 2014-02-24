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
    * type - long (want enum of Strength, Dexterity, Intelligence, Health)
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
  * character/dexterity
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
  * character/dodge
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
  * character/charisma
    * type - long
    * cardinality - one
  * character/odius-personal-habbit
    * type - long
    * cardinality - one
  * character/voice
    * type - long
    * cardinality - one
  * character/wealth
    * type - long (enum of Dead Broke, Poor, Struggling, Average, Comfortable, Very Wealthi, Filthy Rich)
    * cardinality - one
  * character/techology-level (vague, says "Whatever your world's level is" )
    * type - long
    * cardinality - one
  * character/damage-descriptor
    * type - ref (points to Damage Descriptor)
    * cardinality - one
    * component - false (do not want destroy Descriptor if Character is destroyed)
  * character/handedness
    * type - long (want enum of Left, Right, Ambidextrous)
    * cardinality - one
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
  * character/skills
    * type - ref (points to Skill)
    * cardinality - many
    * component - true (destroy Skill if Character is destroyed)

---
## Starting Datomic Environment

1. be in ~/Software/datomic-pro-0.9.4556
2. bin/transactor config/samples/dev-transactor-template.properties
3. bin/console --port 8080 dev datomic:dev://localhost:4334/
4. browse to http://localhost:8080/browse 
5. bin/shell
6. make edits to edn file
7. uri = "datomic:dev://localhost:4334/gurps";
8. Peer.createDatabase(uri); 
9. conn = Peer.connect(uri);
8. schema_rdr = new FileReader("/home/vagrant/GitHub/clojure-stack-experiment/datomic/schema.edn");
8. schema_tx = Util.readAll(schema_rdr).get(0);
9. txResult = conn.transact(schema_tx).get();
10. rinse, repeat
11. data_rdr = new FileReader("/home/vagrant/GitHub/clojure-stack-experiment/datomic/seed-data.edn");
12. data_tx = Util.readAll(data_rdr).get(0); 
13. txResult = conn.transact(data_tx).get(); 

----
## Queries
```clojure
;; Find all entities where :/character/name is equal to "Ronbo"
[:find ?entity
 :where
 [?entity :character/name "Ronbo"]
]
```

## Emacs Notes
1. use [Emcacs PPA](https://launchpad.net/~cassou/+archive/emacs) to get version 24 on Ubuntu, if using an older distro
2. clone [this GitHub repo](https://github.com/flyingmachine/emacs-for-clojure) to your ~/.emacs.d to get a nice environment
3. useful bindings
    * C-g - cancel current action
    * C-x b - create new buffer
    * C-x k - kill buffer
    * C-x C-f - open file
    * C-x C-s - save file
    * C-k - kill rest of line
    * C-/ - undo
    * C-a - move to the beginning of the line
    * M-m - move to the first non-white space in the line
    * C-f - fowared one character
    * C-b - back one character
    * M-f - forward one word
    * M-b - backward one work
    * C-s - search forward
    * C-r - search backwards (reverse)
    * M-< - move to start of buffer
    * M-> - move to end of buffer
    * M-g g - goto specified line number
    * C-space - set mark
    * M-x - replace string
    * M-w - kill-ring-save (copy)
    * M-d - kill-word (cut)
    * C-y - yank (paste)
    * M-y - yank next text in kill ring
    * C-w - kill region
    * C-k - kill line
    * tab - indent
    * C-j - new, indented line
    * M-/ - hippie exanpsion
    * M-\ - delete all space around point
    * C-h k - help on binding
    * C-h f - help on function
    * C-x o q - used to close help windows
    * M-x package-list - list all available packages
    * M-x package-install - install a peraticular package

## REST Services
1. lein new compojure-app server
2. 

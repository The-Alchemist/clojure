# With Vectors

Vectors as the simplest data structure in Clojure to work with.  They are very similar to an array in other languages, although they have additional qualities in Clojure.

Vectors 
* can be of any length
* are indexed so have fast random access
* can contain any types
* are immutable

> Define a data structure for a simple shopping list with any items you would typically want to buy.

<!--sec data-title="Reveal answer" data-id="answer001" data-collapse=true ces-->

```clojure 
(def shopping-list ["Cerial" "Baked Beans" "Cat food" "Quorn chicken pieces" ])
```
<!--endsec-->

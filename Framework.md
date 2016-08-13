# Framework

Het framework is een compleet MVC framework met alles erop en eraan, het grootste gedeelte van de stek draait erop.

 * [Model](Framework-Model)
 * [Entity](Framework-Entity)
 * [View](Framework-View)
 * [Controller](Framework-Controller)

## ORM

In het framework zit een ORM gebouwd, deze is gebasseerd op de ActiveRecord principes. In ORM-wereld zijn er in principe twee manieren om ORM te doen: ActiveRecord of DataMapping. ActiveRecord is simpeler en minder krachtig, Datamapping is moeilijker en krachtiger.

In ActiveRecord zijn objecten zelf verantwoordelijk voor hoe ze opgeslagen worden in de stek gebeurt dit door de abstracte klasse 'PersistentEntity', alle entities extenden deze klasse en kunnen opeens in de database opgeslagen worden. ActiveRecord is super declaratief. Een minimale entity bevat alleen maar definities, zie bijvoorbeeld [`lib/model/entity/Adres.class.php`](https://github.com/csrdelft/csrdelft.nl/blob/7323455ce335c17f1b6cae5de0e33dbd6bbdba9b/lib/model/entity/Adres.class.php). Deze klasse bevat geen enkele logica, alleen maar definities. Een andere klasse, bijvoorbeeld ['lib/model/entity/Peiling.class.php'](https://github.com/csrdelft/csrdelft.nl/blob/7323455ce335c17f1b6cae5de0e33dbd6bbdba9b/lib/model/entity/Peiling.class.php) bevat allerlei logica om de rechten van een gebruiker te controleren en om de relatie met `PeilingStem` goed weer te geven. Zie ook [Entity](Framework-Entity) op de wiki voor meer informatie.

ActiveRecord breekt helaas met [SOLID](https://nl.wikipedia.org/wiki/SOLID) want een Entity is verantwoordelijk voor 'Business Logic' en voor 'Persistence' wat twee verschillende verantwoordelijkheden zijn. Om het toch overzichtelijk te houden wordt in de Entities zelf zo min mogelijk Database gerelateerde zaken gedaan en vooral 'Business Logic'.

Een voorbeeld van een meer die-hard ActiveRecord framework is [PHP ActiveRecord](http://www.phpactiverecord.org/projects/main/wiki/Quick_Start). Dit framework kan alles voor je aannemen! Het is dan genoeg om op de volgende manier een volledige klasse te definieren: `class Adres extends ActiveRecord\Model {}`, en dan heb je en klasse die een tabel adres aanmaakt en automatisch kolommen toevoegt als je het model gebruikt. :astonished:

Overstappen naar een DataMapper ORM kan in de toekomst interessant zijn, omdat dit er voor zorgt dat dingen goed gescheiden blijven. DataMapper kost alleen veel meer code is is soms complexer, het voordeel van een DataMapper is dat er minder magie bij komt kijken.
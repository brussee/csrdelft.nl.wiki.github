Verschillende onderdelen van de stek met hun refactor status

Legenda:

* MVC: Model, view en controller code strikt gescheiden. Controller zo dun mogelijk.
* ACL: Access control list hardcoded of dynamisch.
* DAC: Discretionary access control: toegang op basis van lidmaatschap van een groep.
* PDO: PHP Data Objects door gebruik te maken van Database.class.php.
* ORM: Object-relational mapping door gebruik te maken van PersistentEntity? en PersistenceModel?.
* form: Formulieren niet hardcoden met HTML <form> tag maar gebruikmaken van Formulier.class.php.
* .tpl: Smarty templates om de GUI geheel te scheiden van de PHP code.
* API: Application programming interface: HTML, JSON, mixed. 

| Module              | MVC | ACL | DAC | PDO | ORM | form | .tpl | API |
| ---                 |---|---|---|---|---|---|---| --- |
| Agenda              | v | v | v | v | v | v | v | mixed |
| Courant             |   | v | v |   |   |   | v | mixed |
| Forum               | v | v | v | v | v |   | v | HTML |
| Groepen             | v | v | v | v | v | v |   | HTML |
| Documenten          | v | v | v |   |   | v | v | mixed |
| Login               | v | v | v | v | v | v | v | mixed |
| Profiel             | v | v | v | v | v | v | v | HTML |
| Ledenlijst          |   |   |   |   |   |   |   | mixed |
| Bibliotheek         |   |   |   |   |   |   | v | mixed |
| FotoAlbum           | v | v | v | v | v | v | v | mixed |
| CMS                 | v | v | v | v | v | v |   | HTML |
| Layout              | v |   |   |   |   | v | v | HTML |
| MaalCie             | v | v |   | v |   |   | v | HTML |
| Menu                | v | v | v | v | v | v | v | mixed |
| InstellingenWebstek | v | v |   | v | v |   | v | mixed |
| Peilingen           |   |   |   |   |   |   | v | HTML |
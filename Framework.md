# Framework

Het framework is een compleet MVC framework met alles erop en eraan, het grootste gedeelte van de stek draait erop.

## Model

Een model moet de klasse `PersistenceModel` extenden, een model is de eigenaar van een soort entity. Een model is een singleton en is overal te pakken te krijgen.

De statische attribuut `orm` bevat de entity van dit model.

De protected statische attribuut `$instance` is een verwijzijng naar de singleton instantie.

### Functies

#### `::getUUID($UUID)`

Haalt een entity uit het model gebaseerd op primary key

#### `find($criteria, $criteria_params, ...)`

Zoek entities uit het model gebasseerd op criteria. 

De criteria zijn stiekem de `WHERE` clause in sql, hier kunnen net als in PDO `?`'s gebruikt worden die in de array criteria_params voorkomen.

#### `count($criteria, $criteria_params)`

Aantal entities die voldoen aan de criteria

#### public `exists($criteria, $criteria_params)`

Controleer of er entities bestaan die voldoen aan criteria.

Zelfde als `$model->count($criteria, $criteria_params) > 0`, maar dan sneller.

#### `create($entity)`

Sla de gegeven entity op in de database.

#### `retrieve($entity)`

Haal de entity op uit de database.

Dit is handig om een incomplete entity, met bijvoorbeeld alleen een primary key, aan te vullen. De entity moet tenminste een primary key bevatten.

#### `update($entity)`

Update een entity, gebaseerd op de primary key die de entity bevat.

#### `delete($entity)`

Verwijder de entity uit de database.

### Voorbeeld

```PHP
class VoorbeeldModel extends PersistenceModel {
  const orm = 'Voorbeeld';
  protected static $instance;
}
```
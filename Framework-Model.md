# Model

Een model moet de klasse `PersistenceModel` extenden, een model is de eigenaar van een soort entity. Een model is een singleton en is overal te pakken te krijgen.

De constante `ORM` is de class name van de entity van dit model.

De protected statische attribuut `$instance` is een verwijzijng naar de singleton instantie.

## Functies

### `find($criteria, $criteria_params, ...)`

Zoek entities uit het model gebasseerd op criteria. 

De criteria zijn stiekem de `WHERE` clause in sql, hier kunnen net als in PDO `?`'s en `:var` keys gebruikt worden die in de array criteria_params = array(0 => 'value') of array('var' => 'value') voorkomen.

### `count($criteria, $criteria_params)`

Aantal entities die voldoen aan de criteria

### `exists($criteria, $criteria_params)`

Controleer of er entities bestaan die voldoen aan criteria.

Zelfde als `$model->count($criteria, $criteria_params) > 0`, maar dan sneller.

### `create($entity)`

Sla een nieuwe entity op in de database.

### `retrieve($entity)`

Haal de entity op uit de database.

Dit is handig om een incomplete entity, met bijvoorbeeld alleen een primary key, aan te vullen. De entity moet tenminste een primary key bevatten. Zie ook sparse retrieval.

### `update($entity)`

Update een entity, gebaseerd op de primary key die de entity bevat.

### `delete($entity)`

Verwijder de entity uit de database.

## Voorbeeld

```PHP
require_once 'model/entity/Voorbeeld.class.php';

class VoorbeeldModel extends PersistenceModel {
  const ORM = 'Voorbeeld';
  protected static $instance;
  
  public function get($id) {
    return $this->retrieveByPrimaryKey(array($id));
  }
}
```

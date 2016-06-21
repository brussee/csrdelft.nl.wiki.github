# Framework

Het framework is een compleet MVC framework met alles erop en eraan, het grootste gedeelte van de stek draait erop.

 * [Framework/Model](Model)
 * [Framework/Entity](Entity)
 * [Framework/View](View)
 * [Framework/Controller](Controller)

## Entity

Een entity is een object die data bevat, zoals bijvoorbeeld een mededeling, een bestand, etc.

Een entity moet de klasse PersistentEntity extenden en heeft drie verplichte protected statische variabelen. PersistentEntity bevat enkele functies, maar geen functies die broodnodig zijn. Het is natuurlijk mogelijk om functies in een entity te creeren.

### Variabelen

#### `$table_name`

De naam van de tabel in de database

#### `$persistent_attributes`

Een lijst met attributen van de entity, gemapt naar type.

Type is een array, met de volgende waarden:

* 0: Type uit T (PersistentAttributeType.enum)
* 1: Mag null zijn?
* 2: Als 0 een Enum is, de enum klasse. Anders 'extra', bijvoorbeeld `auto_increment` of comment.

#### `$primary_key`

Een lijst met de volledige primary key, vaak komt dit neer op `array('id')`.

### Voorbeeld
```PHP
class Voorbeeld extends PersistentEntity {
  public $id;
  public $voornaam;
  public $achternaam;
  public $verhaal;

  public volledigeNaam() {
    if ($achternaam != '') {
      return $voornaam . ' ' . $achternaam;
    } else {
      return $voornaam;
    }
  }

  protected static $table_name = 'voorbeelden';
  protected static $persistent_attributes = array(
    'id'         => array(T::Integer, false, 'auto_increment'),
    'voornaam'   => array(T::String),
    'achternaam' => array(T::String, true),
    'verhaal'    => array(T::Text)
  );
  protected static $primary_key = array('id');
}
```
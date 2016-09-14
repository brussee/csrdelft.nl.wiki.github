# Entity

Een entity is een object dat data bevat, zoals bijvoorbeeld een mededeling, een bestand, etc.

Een entity die je in de database wilt opslaan moet de klasse PersistentEntity extenden en heeft drie verplichte protected statische variabelen. PersistentEntity mag alleen functies bevatten die relevant zijn voor dit ene object, business logic voor meerdere entities tegelijkertijd moeten in PersistenceModel, vandaar ook de enkelvoud vs. meervoud namen.

## Variabelen

### `$table_name`

De naam van de tabel in de database

### `$persistent_attributes`

Een lijst met attributen van de entity, gemapt naar type.

Type is een array, met de volgende waarden:

* 0: Type uit T (PersistentAttributeType.enum)
* 1: Mag null zijn?
* 2: Als 0 een Enum is, de enum klasse. Anders 'extra', bijvoorbeeld `auto_increment` of comment.

### `$primary_key`

Een lijst met de volledige primary key, vaak komt dit neer op `array('id')`.

## Voorbeeld
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
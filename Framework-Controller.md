# Controller

In de controller komt alles bij elkaar, deze hangt namelijk [Models](Framework-Model) aan [Views](Framework-View) gebasseerd op de gegeven route. Een controller wordt vanuit `htdocs/index.php` aangeroepen en is dus het eerste wat er tegen wordt gekomen.

Er zijn twee smaakjes controller in het framework, de `AclController` en de `Controller`. De AclController is het makkelijkst om te gebruiken.

## Functies

### `__construct($query)`

De query variabele bevat alle data van de opgevraagde url, deze moet doorgespeeld worden naar `parent::__construct`, samen met de model die gebruikt wordt. De eerste regel van `__construct` is dus altijd in de vorm `parent::__construct($query, EenSoortModel::instance());`

Hierna wordt de acl (Acces Control List) ingesteld, hier wordt per functie van de controller aangegeven wie het mag doen. Dit is een ingewikkeld process (zie AccessModel.class.php). De woorden in `AccessModel->permissions` kunnen worden gebruikt (AccessModel.class.php vanaf regel 281).

Deze acl is een array met als key de functie in controller en als waarde de permissie die nodig is en wordt opgeslagen in `$this->acl`. `$this->isPosted()` is handig om te kijken of de request een post request is, op deze manier kan 1 pagina opslaan en bekijken tegelijk, waarbij voor opslaan andere rechten nodig zijn.

Als een doel niet in acl gevonden wordt, wordt een 404 pagina aan de eindgebruiker gepresenteerd.

### `performAction(array $args)`

Dit is de functie die door de controller aangeroepen wordt en kan worden gebruikt om de parameters van de request door te pluizen. In deze functie moet `$this->action` gezet worden, dit is een string die overeen komt met de functie die aangeroepen wordt.

Aan het eind moet `parent::performAction(array)` uitgevoerd worden, deze zorgt ervoor dat de functie in `$this->action` uitgevoerd wordt met de waarden die aan de parent worden meegegeven.

Functies die in performAction te gebruiken zijn om de parameters te bekijken zijn `$this->getParam($key)` en `$this->getParams($num)`, om respectievelijk de `$key`de parameter op te vragen of alle parameters na `$num` op te vragen.

### Verzin je functie

De functie die parent::performAction uitvoert moet er uiteindelijk voor zorgen dat `$this->view` een view bevat, deze wordt dan uiteindelijk gerendered.

## Voorbeeld
```PHP
require_once 'model/VoorbeeldModel.class.php';
require_once 'view/VoorbeeldView.class.php';

class VoorbeeldController extends AclController {
  public function __construct($query) {
    parent::__construct($query, VoorbeeldModel::instance());
    if (!this->isPosted()) {
      $this->acl = array(
        'lijst' => 'P_VOORBEELD_READ',
        'bekijken' => 'P_VOORBEELD_DETAIL'
      );
    } else {
      $this->acl = array(); // Mag niet posten
    }
  }

  public function performAction(array $args = array()) {
    $this->action = 'lijst';
    if ($this->hasParam(1)) {
      $this->action = 'bekijken';
      $args = $this->getParam(1);
    }
    $body = parent::performAction($args);
    $this->view = new CsrLayoutPage($body);
  }

  public function lijst() {
    return new VoorbeeldenView($this->model);
  }

  public function bekijken($id) {
    return new VoorbeeldView($id, $this->model);
  }
}
```

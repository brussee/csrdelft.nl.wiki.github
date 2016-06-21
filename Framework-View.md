# View

Een view is een _implementatie_ van View, een view krijgt een [Model](Framework-Model) toegeworpen en laat deze zien. In principe gebruikt een view slechts wat hij krijgt van de controller en niets anders, op sommige plekken is dit helaas niet mogelijk.

Als er voor wordt gekozen om `SmartyTemplateView` te extenden hoeft alleen `view()` geimplementeerd te worden en kan smarty gebruikt worden, dit is aanbevolen.

## Smarty

Als `SmartyTemplateView` ge-extend wordt, geeft deze de variabele `$this->smarty` wat een smarty instantie is.

De SmartyTemplateView constructor heeft een [Model](Framework-Model) en een titel als argumenten.

### Smarty gebruik

De variabele `$this->smarty` heeft twee handige functies, `assign` om een variabele aan de smarty template toe te voegen en `display` om een template te laten zien. Deze templates staan in lib/templates.

## Te implementeren bij `View`

### `__construct($model)`

Ontvang een model van de controller, sla deze op in `$this->model`.

### `getTitel()`

Uitkomst hiervan wordt gebruikt voor de `<title>`.

### `getBreadCrumbs()`

Uitkomst hiervan wordt in de breadcrumb bar geshowd.

### `getModel()`

Om het model beschikbaar te maken.

### `view()`

Deze functie is het belangrijkst, hier wordt een string __geprint__ die uiteindelijk op het scherm verschijnt.

Alles wat hier met `echo` of `print` wordt geprint word direct naar de output buffer geschreven.

## Voorbeeld

```PHP
class VoorbeeldenView extends SmartyTemplateView {
  public function __construct($model) {
    parent::__construct($model, 'Voorbeelden overzicht');
  }

  public function view() {
    $this->smarty->assign('voorbeelden', $this->model->find());
    $this->smarty->display('voorbeelden/voorbeelden.tpl');
  }
}

class VoorbeeldView extends SmartyTemplateView {
  public function __construct($id, $model) {
    parent::__construct($model, 'Voorbeeld');
    $this->id = $id;
  }

  public function view() {
    $this->smarty->assign('voorbeeld', $this->model->get($id));
    $this->smarty->display('voorbeelden/voorbeeld.tpl');
  }
}
```

### `voorbeelden/voorbeelden.tpl`
```smarty
<h1>Voorbeelden</h1>
{foreach from=$voorbeelden item=voorbeeld}
  <h2><a href="voorbeelden/{$voorbeeld->id}">{$voorbeeld->volledigeNaam()}</a></h2>
{/foreach}
```

### `voorbeelden/voorbeeld.tpl`
```smarty
<h1>{$voorbeeld->volledigeNaam()}</h1>
<p>{$voorbeeld->verhaal}</p>
```
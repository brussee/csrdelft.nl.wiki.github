# [WIP!] Formulier

Formulier is een klasse die gebruikt kan worden om formulieren op te bouwen en te valideren. Alle bestanden gerelateerd aan Formulier zijn te vinden in `lib/view/formulier`. Een formulier kan een hele pagina in beslag nemen, in een smarty template voorkomen of als popup gebruikt worden.

## Fields

Velden zijn te vinden in `FormElement.abstract.php`, `FormKnoppen.class.php`, `GetalVelden.class.php`, `InvoerVelden.class.php`, `KeuzeVelden.class.php` en `UploadVelden.class.php`.

<table>
<tr>
  <th>Veld</th>
  <th>Invoer</th>
  <th>Beschrijving</th>
  <th>Voorbeeld</th>
</tr>
<tr>
  <th colspan="4">FormElement</th>
</tr>
<tr>
  <td>HtmlComment</td>
  <td>$comment</td>
  <td>Schrijft ingevoerde HTML direct naar output</td>
  <td><code>new HtmlComment('&lt;p&gt;Dit formulier gaat je leven verder helpen.&lt;/p&gt;');</code></td>
</tr>
<tr>
  <td>HtmlBbComment</td>
  <td>$comment</td>
  <td>Schrijf ingevoerde HTML via de BB parser naar output</td>
  <td><code>new HtmlBbComment('[b]Belangrijk[/b]');</code></td>
</tr>
<tr>
  <td>FieldSet</td>
  <td>$comment</td>
  <td>Maak een nieuwe fieldset aan met $comment als titel, de FieldSet moet nog wel gesloten worden met &lt;/fieldset&gt;</td>
  <td><code>new FieldSet('Stuff');</code></td>
</tr>
<tr>
  <td>SubKopje</td>
  <td>$comment</td>
  <td>Maak een subkopje aan, de standaard is h3. De eigenschap $h is publiek.</td>
  <td><code>$s = new SubKopje('Vier'); $s->h = 4;</code></td>
</tr>
<tr>
  <td>CollapsableSubKopje</td>
  <td>$id, $titel, $collapsed = false, $single = false, $hover_click = false, $animate = true</td>
  <td>Zelfde als subkopje, maar bij klik verschijnt/verdwijnt alles tot 
</table>


|HtmlComment | $html | |
|HtmlBbComment
|FieldSet
|Subkopje
|CollapsableSubKopje
|
|(Required)TextField
|(Required)FileNameField
|(Required)LandField
|(Required)RechtenField
|(Required)LidField
|(Required)EntityField
|StudieField
|(Required)EmailField
|(Required)UrlField
|(Required)UsernameField
|(Required)DuckField
|(Required)TextareaField
|(Required)WachtwoordField
|(Required)WachtwoordWijzigenField
|
|(Required)IntField
|(Required)BedragField
|(Required)TelefoonField
|(Required)FloatField
|
|(Required)ColorField
|(Required)SelectField
|MultiSelectField
|(Required)EntityDropDown
|(Required)WeekdagField
|(Required)VerticaleField
|(Required)KerkField
|(Required)RadioField
|(Required)GeslachtField
|(Required)JaNeeField
|(Required)DateField
|(Required)TimeField
|(Required)CheckboxField
|(Required)DateTimeField
|(Required)SterrenField
|
|(Required)FileField
|(Required)ImageField
|BestandBehouden
|UploadFileField
|ExistingFileField
|DownloadUrlField



## Voorbeeld 

```PHP
class MijnForm extends Forumulier {
    function __construct(Voorbeeld $model) {
        parent::__construct($model, '/voorbeeld/', false, true);
        $fields[] = new RequiredTextField('voornaam', $model->voornaam, 'Voornaam');
        $fields[] = new TextField('achternaam', $model->voornaam, 'Achternaam');
        $fields[] = new RequiredTextField('verhaal', $model->verhaal, 'Verhaal');
        $fields['btn'] = new FormDefaultKnoppen();

        $this->addFields($fields);
    }
}
```

```PHP
class VoorbeeldController extends Controller {
    ...

    public function voorbeeld() {
        if ($this->isPosted()) {
            $voorbeeld = new Voorbeeld();
            $voorbeeld->voornaam = filter_input(INPUT_POST, 'voornaam', FILTER_SANITIZE_STRING);
            $voorbeeld->achternaam = filter_input(INPUT_POST, 'achternaam', FILTER_SANITIZE_STRING);
            $voorbeeld->verhaal = filter_input(INPUT_POST, 'verhaal', FILTER_SANITIZE_STRING);
            $form = new MijnForm($voorbeeld);
            if ($form->validate()) {
                VoorbeeldModel::instance()->create($voorbeeld);
                setMelding("Voorbeeld met succes aangemaakt", 1);
                $this->view = new MijnForm(new Voorbeeld());
            } else {
                $this->view = $form;
            }
        } else {
            $this->view = new MijnForm(new Voorbeeld());
        }
    }
}
```
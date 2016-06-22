# Smarty
Een template systeem te vinden op http://www.smarty.net/.

De zelfgeschreven plugins zijn te vinden in [`lib/smarty_plugins`](/csrdelft/csrdelft.nl/lib/smarty_plugins)

Een korte introductie: [http://www.smarty.net/crash_course] en hieronder.

## Voorbeelden
Hier onder een klein rijtje mogelijkheden om een idee te geven van het template systeem.

### Conversies

```smarty
{$tekstmetubbtags|bbcode}
{$maaltijd.datum|date_format:$datumVol}
{$maaltijd.tekst|truncate:20|escape:'html'}
{$smarty.now}
{$smarty.session}
```
### globale variabelen
```smarty
{$CSR_PICS} is CSR_PICS
```

### defineren van lokale variabelen
source:/trunk/lib/groepen/groepcontent.class.php#L161
```PHP
public function view(){
	$smarty = CsrSmarty::instance();

	$smarty->assign('groepen', $this->groepen);

	$smarty->assign('action', $this->action);
	$smarty->assign('gtype', $this->groepen->getNaam());
	$smarty->assign('groeptypes', Groepen::getGroeptypes());

	$smarty->assign('melding', $this->getMelding());
	$smarty->display('groepen/groepen.tpl');
}
```

Bijvoorbeeld te gebruiken als volgt:
```smarty
...
{foreach from=$groepen->getGroepen() item=groep}
	<div class="groep clear" id="groep{$groep->getId()}">
		<div class="groepleden">
			{if $groep->toonPasfotos()}
				{assign var='actie' value='pasfotos'}
			{/if}
			{include file='groepen/groepleden.tpl'}
		</div>
		<h2><a href="/actueel/groepen/{$groepen->getNaam()}/{$groep->getId()}/">{$groep->getNaam()}</a></h2>
		{if $groep->getType()->getId()==11 }Ouderejaars: {$groep->getEigenaar()|perm2string}<br />{/if} {* alleen bij Sjaarsacties *}
		{$groep->getSbeschrijving()|ubb}
	</div>
{/foreach}
...
```
Variabelen in template defineren
```smarty
{assign var='actief' value='instellingen'}
```

Logische elementen
```smarty
{foreach from=$array item=eenarrayitem}
    {$eenarrayitem}
{/foreach}


{if voorwaarde}{/if}
{if voorwaarde}{else}{/if}
{if voorwaarde}{elseif voorwaarde2}{/if}

style="background-color: {cycle values="#e9e9e9, #fff"}">
```

Andere template invoegen:
```smarty
{include file='groepen/groepformulier.tpl'}
```

Commentaar
```smarty
{* 	commentaar en dat
     mag over meer regels 
	 *}
```

Of code letterlijk nemen zonder smarty interpretatie (niet meer nodig zolang er maar een whitespace achter de openingsbracket staat)
```smarty
<script type="text/javascript">
	{literal}
	if(window.location.hash!=''){
		showTab('{/literal}{$groep->getId()}{literal}', window.location.hash.substring(1));
	}else{
		{/literal}
		{if $groep->toonPasfotos()}
			showTab('{$groep->getId()}', 'pasfotos');
		{/if}
		{literal}
	}
	{/literal}
</script>
```

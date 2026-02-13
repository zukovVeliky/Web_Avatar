# Web Avatar (Razor Pages)

Znovupouzitelna avatar komponenta s orezem obrazku, navazana na `Web_FileManager` picker.

## Zasadni informace

- Avatar editor otevira obrazky pres `/FileManager`.
- Bez funkcniho `Web_FileManager` nelze vybirat vstupni obrazky.
- Vysledky orezu se ukladaji do textoveho logu (`App_Data/avatar-crop-records.txt`).

## Obsah repozitare

- `Areas/Avatar/EditaceAvatara.cshtml`
- `Areas/Avatar/EditaceAvatara.cshtml.cs`
- `Areas/Avatar/AvatarCropp.cs`
- `Areas/Avatar/AvatarUmisteniIMG.cs`
- `wwwroot/lib/Avatar/croppie.js`
- `wwwroot/lib/Avatar/croppie.css`
- `wwwroot/lib/Avatar/EditaceAvatara.js`
- `wwwroot/Images/Foto_Avatar/1000.jpg`

Repo neobsahuje hotovou cilovou stranku. Obsahuje pouze komponentove soubory a manual pro vlozeni do vlastni stranky.

## Pozadavky

- ASP.NET Core Razor Pages
- komponenta `Web_FileManager`
- nastaveni `Avatar` sekce v `components.settings.json` (z `Web_FileManager`)
- NuGet balicek `System.Drawing.Common` (pro server-side crop)

## Instalace

1. Naklonuj:
   - `git clone https://github.com/zukovVeliky/Web_Avatar.git`
2. Zkopiruj soubory do ciloveho projektu pri zachovani cest.
3. Ujisti se, ze v layoutu je dostupny Bootstrap (volitelne).
4. Over, ze projekt ma dostupne:
   - `WebApplication1.Services.ComponentSettings.*` (z `Web_FileManager`)
   - `/FileManager` route (z `Web_FileManager`)
5. Vytvor si vlastni Razor Page (nebo partial), kde Avatar vlozis.

## Konfigurace

Avatar cte konfiguraci z `components.settings.json`:

- `instanceId`: `avatar-main`
- `pageScope`: `/AvatarFileManager`

Klicove volby:

- `cropWidth`, `cropHeight`: vystupni orez
- `maxCropStateLength`: limit hidden crop payloadu
- `maxDisplayedLogEntries`: limit zobrazenych log zaznamu
- `fileManagerOnlyImages`, `fileManagerAllowExt`: omezeni pickeru
- `fileManagerUseDefaultRoot`, `fileManagerRoot`: root slozka pickeru

## Vazba na FileManager

Avatar stranka otvira:

- `/FileManager?picker=1&onlyImages=1&allowExt=...&callback=...`

FileManager vrati URL obrazku callbackem:

- `window.ZachyceniURL_<modifier>(avatarUrl)`

Nad tim pak probiha orez a ulozeni vystupu.

## Nasazeni

- aplikace musi mit zapis do:
  - `wwwroot/Images/Foto_Avatar`
  - `wwwroot/Images/Foto_Avatar/Cropped`
  - `App_Data/avatar-crop-records.txt`
- pro Linux host je nutne nahradit `System.Drawing` (nebo zajistit kompatibilitu)

## Troubleshooting

- nelze vybrat obrazek:
  - nebezi `/FileManager`
  - popup blocker
- orez nelze ulozit:
  - chybi zapisova prava
  - nevalidni crop payload
- obrazek po ulozeni chybi:
  - blokovane staticke soubory / spatna URL base

## Vlozeni do vlastni stranky (manual)

1. V `PageModel` vytvor `EditaceAvataraModel`.
2. Do `OnGet` (a `OnPost`) nacitej nastaveni a inicializuj model.
3. V `.cshtml` vykresli partial:
   - `@await Html.PartialAsync("~/Areas/Avatar/EditaceAvatara.cshtml", Model.AvatarEditor)`
4. Pridej tlacitko, ktere otevre `/FileManager?picker=1...` s callbackem `ZachyceniURL_<modifier>`.
5. Pri `OnPost` precti hidden field `HiddenParametry_<modifier>` a zavolej:
   - `AvatarCropp.GetNewAvatar(...)`

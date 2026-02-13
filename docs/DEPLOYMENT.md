# Avatar Deployment Checklist

## 1. Required dependency

- `Web_FileManager` je nainstalovan a bezi na `/FileManager`.

## 2. Required files

- `Pages/AvatarFileManager.cshtml(.cs)` (jen demo, volitelne)
- `Areas/Avatar/*`
- `wwwroot/lib/Avatar/*`
- fallback image `wwwroot/Images/Foto_Avatar/1000.jpg`
- vlastni Razor Page, ktera Avatar komponentu vykresli

## 3. Required config

- `components.settings.json` obsahuje `components.avatar.instances[]` pro:
  - `instanceId = avatar-main`
  - `pageScope = /AvatarFileManager`

## 4. Runtime permissions

- write access:
  - `wwwroot/Images/Foto_Avatar/Cropped`
  - `App_Data`

## 5. Smoke test

1. otevri svoji stranku s Avatar komponentou
2. klikni vlastni tlacitko `Vybrat obrazek z FileManageru`
3. vyber obrazek
4. proved orez (Ctrl + drag/zoom)
5. uloz
6. over zaznam v seznamu a nahled ulozeneho obrazku

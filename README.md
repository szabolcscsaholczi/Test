# 0. rész – Áttekintés

Kezdjük egy alapvető áttekintéssel a .NET MAUI-ról és arról, hogyan épülnek fel a projektek.

## Megoldás megnyitása a Visual Studio-ban

1. Nyisd meg a **1. rész – Adatok megjelenítése/MonkeyFinder.sln** fájlt.

Ez a MonkeyFinder 1 projektet tartalmaz:

- **MonkeyFinder** – A fő .NET MAUI projekt, amely Android, iOS, macOS és Windows célokra épül. Tartalmazza az alkalmazás összes keretét, beleértve a modelleket, nézeteket, nézetmodelleket és szolgáltatásokat.

![A majomkereső alkalmazás megoldása, több mappával](https://github.com/szabolcscsaholczi/Test/blob/main/Art/Solution.PNG)

A **MonkeyFinder** projekt emellett üres kódfájlokat és XAML oldalakat is tartalmaz, amelyeket a workshop során fogunk használni. A módosított kódok mind ebben a projektben lesznek.

## A .NET MAUI egyprojektes megoldásának megértése

A .NET Multi-platform App UI (.NET MAUI) egyprojektes megoldása magába foglalja azokat a platform-specifikus fejlesztési tapasztalatokat, amelyekkel általában találkozol alkalmazások készítésekor, és ezeket egyetlen közös projektbe absztrahálja, amely Android, iOS, macOS és Windows célokra építhető.

Az egyprojektes megoldás egyszerűsített és egységes, platformok közötti fejlesztési élményt nyújt, függetlenül a célplatformoktól. A .NET MAUI egyprojektes megoldás a következő funkciókat kínálja:

- Egyetlen közös projekt, amely Android, iOS, macOS és Windows célokra építhető.
- Egyszerűsített hibakeresési cél kiválasztás a .NET MAUI alkalmazások futtatásához.
- Közös erőforrásfájlok az egy projektben.
- Hozzáférés a platform-specifikus API-khoz és eszközökhöz, amikor szükséges.
- Egyetlen, platformok közötti alkalmazásindító pont.

A .NET MAUI egyprojektes megoldása a multi-targeting (többcélú fejlesztés) és a .NET 6 SDK-stílusú projektjeinek használatával valósul meg.

### Erőforrásfájlok

A keresztplatformos alkalmazásfejlesztés erőforrásainak kezelése hagyományosan problémás volt. Minden platformnak megvan a saját erőforrás-kezelési módszere, amelyet külön-külön kell megvalósítani. Például, minden platform eltérő képkövetelményeket támaszt, ami tipikusan azt jelenti, hogy minden képből több verziót kell készíteni különböző felbontásokban. Így egyetlen képet általában többször kell másolni platformonként, különböző felbontásokban, és az így kapott képeknek különböző fájlnév- és mappakonvenciókat kell követniük.

A .NET MAUI egyprojektes megoldása lehetővé teszi, hogy az erőforrásfájlokat egyetlen helyen tárold, miközben minden platformon felhasználásra kerülnek. Ide tartoznak a betűtípusok, képek, az alkalmazás ikonja, a splash képernyő és a nyers eszközök.

> **FONTOS:**  
> Minden kép erőforrásfájl forrásképként szolgál, amelyből a build folyamat során generálódnak a szükséges felbontású képek minden platformra.

Az erőforrásfájlokat a .NET MAUI alkalmazás projekted _Resources_ mappájába, vagy annak al-mappáiba kell helyezni, és megfelelő build akciót kell beállítani számukra. Az alábbi táblázat bemutatja az egyes erőforrásfájl típusokhoz tartozó build akciókat:

| Erőforrás        | Build akció       |
| ---------------- | ----------------- |
| Alkalmazás ikon  | MauiIcon          |
| Betűtípusok      | MauiFont          |
| Képek            | MauiImage         |
| Splash képernyő  | MauiSplashScreen  |
| Nyers eszközök   | MauiAsset         |

> **MEGJEGYZÉS:**  
> A XAML fájlok szintén a .NET MAUI alkalmazás projektedben vannak, és amikor a projekt- vagy elem sablonok által létrehozásra kerülnek, automatikusan **MauiXaml** build akciót kapnak. Azonban a XAML fájlok általában nem a _Resources_ mappában találhatók.

Amikor egy erőforrásfájl hozzáadásra kerül egy .NET MAUI alkalmazás projekthez, a projekt (.csproj) fájlban létrejön egy megfelelő bejegyzés az adott erőforrás számára. A fájl hozzáadása után a build akciót a **Tulajdonságok** ablakban tudod beállítani. Az alábbi képernyőkép egy _Resources_ mappát mutat, amely al-mappákban tartalmaz képeket és betűtípusokat:

![Képek és betűtípus erőforrások képernyőképe](https://github.com/szabolcscsaholczi/Test/blob/main/Art/ResourcesSingleProject.png)

A _Resources_ mappa al-mappái egyes erőforrás típusok számára úgy rendelhetők hozzá, ha szerkeszted az alkalmazás projektjének fájlját:

```xml
<ItemGroup>
    <!-- Képek -->
    <MauiImage Include="Resources\Images\*" />

    <!-- Betűtípusok -->
    <MauiFont Include="Resources\Fonts\*" />

    <!-- Nyers eszközök (távolítsd el a "Resources\Raw" előtagot) -->
    <MauiAsset Include="Resources\Raw\**" LogicalName="%(RecursiveDir)%(Filename)%(Extension)" />
</ItemGroup>

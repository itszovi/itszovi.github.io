# WordPress

## Frissítések, verziókezelés és költöztetés

A WordPressre épülő oldalak elterjedtségük miatt is nagyon gyakran támadottak és az egész third-party plugin ökoszisztéma miatt sok rengeteg exploit. Ezeket állandó WordPress és plugin frissítésekkel kell kordában tartani. Érdemes olyan pluginokat használni amelyek állandóan jönnek ki új verziók, fejlesztőik grantálják, hogy működnek az aktuális legfrissebb WP verzióval.

A WordPress oldalak verziókezelése nem könnyű feladat de szerencsére kényelmetlen és körülményes. Ez főleg akkor igaz ha nem VPS-en vagy saját szerveren hanem osztott tárhelyen fog futni az adott WordPress oldal. A legtöbb megosztott tárhelyhez a szolgáltató nem ad (vagy max fizetős extra szolgáltatásként) SSH elérést vagy valamilyen Git integrációt ezért kénytelenek vagyunk FTP-n keresztül feltölteni az oldalt.

Új telepítésnél a legfrissebb WordPress [letöltése](https://wordpress.org/download/) ajánlott de ha valami miatt egy korábbi adott verzióra van szükségünk azokat is le lehet tölteni [ezen az oldalon](https://wordpress.org/download/releases/).

A core mappákon és az adatbázison és fájlokon kívül egy Wordpress oldal költöztetéséhez az mappák szükségesek:

* /themes - A témá(i)nk mappáját tartalmazza. 
* /plugins - A témáink által használt pluginok mappája
* /wp-content/uploads - Ez a mappa tartalmazza a feltöltött képeket, videókat és minden egyéb fájlt amit a Media Libraryn keresztül töltünk fel az admin felületen.

### Git repo

A projekt git repojába nem érdemes az egész core WordPresst feltölteni. **Fontos, hogy a gyökérkönyvtárban található wp-config.php tartalmazza az adatbázis elérését, felhasználónevét és jelszavát úgyhogy ezt ha lehet egy privát repoba se töltsük fel.** Érdemes a github saját WordPresses gitignore [fájlját](https://github.com/github/gitignore/blob/main/WordPress.gitignore) használni vagy legalább abból [kindulni](https://gist.github.com/samhotchkiss/6190462).

Gitre a saját témánk mappáját és a használt pluginokat érdemes feltölteni, főleg ha saját pluginokat használunk.

### Adatbázis exportálás

Mivel a WordPress adatbázisban minden url abszolút ezért költöztetésnél ezeket át kell írnunk, hogy az új domainre mutassanak. Manuálisan SQL parancsokkal is meg lehet oldani de elég fájdalmas tud lenni a rengeteg szerializált adat miatt. A migrálásokhoz érdemes feltelepíteni az [All-in-One WP Migration](https://wordpress.org/plugins/all-in-one-wp-migration/) amivel ezek a problémák egyszerűen megoldhatóak.

Az All-in-One WP Migration pluginnal exportálásnál megadhatjuk a jelenlegi és jövőbeli domaineket, a plugin ezeket az adatbázisban kicseréli azokon a helyeken is ahol manuálisan nehezebb lenne.

Ugyanitt érdemes kiválasztani, hogy csak az sql részeket exportáljuk.

![Exportálás beállítása](/img/export.png)

A teljes folyamat valahogy így néz ki:

1. WordPress core telepítése a tárelyre
2. Uploads, themes és plugins mappa felmásolása a local/teszt környezetről az élesre
3. Bejelentkezés az admin felületre és All-in-One WP Migration plugin telepítése
4. Local/teszt környezetből az adatbázis exportálása fenti beállításokkal
5. Az adatbázis importálása az frissen telepített oldalon
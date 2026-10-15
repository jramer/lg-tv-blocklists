# LG-TV: egna domänlistor

Förberedda 2026-10-15 för LG OLED77C4LA C4 och LG OLED55CX3LA CX.

## Innehåll

- `lg-tv-base.txt`: tre observerade värdar med exakta träffar i LG TV Blocklists SAFE-lista. Börja med denna lista på båda TV-apparaterna. Underlagets funktionstester gäller främst G1; funktionen på C4 och CX behöver testas.
- `lg-tv-test.txt`: en separat svensk annonskandidat. Domännamnet talar för annonsfunktion, men säker blockering på C4/CX är inte verifierad. Aktivera separat efter att baslistan har testats.

Listorna består av rena domännamn, ett per rad, utan kommentarer, IP-adresser eller jokertecken.

SDP-, rdx2-, IBS- och okända API-värdar ingår inte. Särskilt regionala SDP-värdar kan behövas för klocka och appar efter kallstart.

## Hosting

Listorna finns på https://github.com/jramer/lg-tv-blocklists. Använd följande Raw-adresser i UniFi:

- Baslista: https://raw.githubusercontent.com/jramer/lg-tv-blocklists/main/lg-tv-base.txt
- Testlista: https://raw.githubusercontent.com/jramer/lg-tv-blocklists/main/lg-tv-test.txt

Ändra innehållet i samma fil när listan ska ändras; GitHub behåller ändringshistorik och gör återställning möjlig. Listorna hämtar inte automatiskt nya domäner från originalprojektet.

CSV-exporter och uppgifter om hemnätet behövs inte i repositoryt.

## UniFi

External Network Lists finns i Network 11; funktionen kräver UniFi OS 6.0.10 eller senare enligt Ubiquitis versionsinformation för Network 11.0.83. Kontrollera installerade versioner och att alternativet External finns.

1. Skapa två domänlistor i Network Lists med External som källa och respektive publicerad Raw-adress.
2. Välj daglig hämtning om alternativet finns.
3. Skapa en grupp med enbart de TV-apparater som ska omfattas.
4. Skapa en blockeringspolicy med dessa TV-apparater som källa och baslistan som destination. Om IP-version är valbar, inkludera både IPv4 och IPv6. Aktivera loggning för att kunna verifiera träffar.
5. Behåll testlistans blockeringspolicy avstängd under första testet.
6. Kontrollera att båda TV-apparaternas DNS-frågor går via gatewayens resolver. Att en lista har importerats visar inte i sig att all trafik till värdarna blockeras.

Exakta menyval och tillgängliga policyfält behöver kontrolleras i den installerade versionen.

## Kontroll och återställning

Verifiera att listan har hämtats och att domänerna blockeras för TV-apparaterna. Testa därefter LG Content Store, appuppdatering, kontroll efter firmwareuppdatering, Netflix, YouTube, Prime Video, SVT Play och TV4 Play i den mån de används. Testa också korrekt tid och appstart efter kallstart.

Aktivera sedan testlistan separat, gärna på en TV först, och upprepa relevanta funktionstester. Vid problem: stäng av den senast aktiverade policyn direkt. Ändringar av en hostad lista kan dröja tills nästa hämtning och tills DNS-cache eller befintliga anslutningar upphör.

## Källor och licens

Baslistans tre domäner är ett urval ur **LG TV Blocklist**, furkan-bayrak och projektets bidragsgivare:

- Projekt och upphov: https://github.com/furkan-bayrak/lg-tv-blocklist
- Annoterad SAFE-källa: https://github.com/furkan-bayrak/lg-tv-blocklist/blob/main/src/safe.txt
- Licens för listinnehållet: Creative Commons Attribution 4.0 International, https://creativecommons.org/licenses/by/4.0/
- Anpassning: urval av tre värdar observerade i användarens UniFi-exporter; ingen automatisk synkronisering av framtida tillägg från originalprojektet.

Testlistans domän är en kandidat identifierad i användarens export. Trafikloggar visar inte tjänstens innehåll eller att blockering är utan funktionspåverkan.

Ubiquitis versionsinformation: https://community.ui.com/releases/UniFi-Network-Application-11-0-83/d6940379-cd5a-4e61-8596-7e22d647a4ea

Rapporter om SDP, klocka och appar: https://github.com/hagezi/dns-blocklists/issues/11438

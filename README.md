# Google Ads — konto Marotino (846-403-8850)

Dokumentacja konta Google Ads **846-403-8850** ("Marotino", waluta EUR, kraj Cyprus, pod managerem 331-108-1372). Ten plik jest napisany tak, żeby ktoś (człowiek albo agent) mógł zrozumieć od zera co jest ustawione, dlaczego, i na co uważać — bez odtwarzania całego procesu decyzyjnego.

Na koncie żyją **dwie kampanie**:

| Kampania | Co promuje | Geo | Status |
|---|---|---|---|
| **Xenia - Hotel AI Receptionist - FL Search** | produkt Xenia (AI recepcjonista dla hoteli) | Floryda / East Coast US | **Paused** od 14.09.2026 — zero realnych leadów |
| **Marotino - Integrations & Ecommerce - US Search** | Marotino jako firma / usługi software house | **całe USA** + korekta stawek na Miami DMA | **W budowie** (14.09.2026), draft, jeszcze nie uruchomiona |

Większość tego pliku to historia kampanii Xenia — to nie jest archiwum dla samego archiwum, tylko **zapis pięciu kolejnych „prawdziwych przyczyn" zerowego serwowania**, z których każda wyglądała identycznie z zewnątrz. Nowa kampania korzysta z tych wniosków bezpośrednio (patrz sekcja o kampanii lead-gen niżej).

Wspólne dla obu kampanii: konto, billing (profil **Marotino CY LTD**, Cyprus, VAT CY60017620T, Postpay), tag Google Ads **AW-18418762437** w kontenerze GTM-5JRBQF9N, oraz property GA4 **G-JT28P4C0LD**.

---

## Kampania 1: Xenia — AI recepcjonista dla hoteli

Kampania dla produktu **Xenia** (`marotino.com/products/xenia`, po zmianie slugu `/products/xenia-white-label-mobile-app-rag`).

Uruchomiona: **30.08.2026**. Zapauzowana: **14.09.2026**.

### TL;DR — stan na dziś

- Konto Google Ads: **846-403-8850**, nazwa konta "Marotino", waluta **EUR**, kraj **Cyprus**.
- Kampania: **"Xenia - Hotel AI Receptionist - FL Search"**, typ **Search only** (bez Display/PMax — brak assetów graficznych na start).
- Geo: **Floryda, USA** (nie global, nie Polska).
- Budżet: **€20/dzień** (świadomy smoke test, nie docelowy budżet).
- Cel konwersji: **Submit lead form** — realny formularz na stronie (`xenia-pilot`, Netlify Forms), nie telefon.
- Billing: profil płatności **Marotino CY LTD** (Cyprus, VAT CY60017620T), Postpay, karta Visa …9422.
- Status: **Paused od 14.09.2026** (wcześniej Enabled od 30.08). Wyniki 1-7.09: 1620 impr., 80 kliknięć, CTR 4,94%, śr. CPC €1,90, koszt €151,99.
- Konwersje: **przeprojektowane na server-side** (05.09.2026) — patrz sekcja niżej. GA4 (Measurement Protocol) działa niezależnie; upload do Google Ads nigdy nie ruszył (patrz aktualizacja o developer tokenie z 14.09.2026 niżej).
- **14.09.2026 — kampania ZAPAUZOWANA.** Powód: tydzień 7-13.09 dał **1247 impresji, 146 kliknięć, €132.18 kosztu, śr. CPC €0.91, CTR 11.71%** — i **zero realnych leadów**. Google Ads raportował "2.00 konwersje" (conv. rate 1.37%, koszt/konw. €66.09), ale to niemal na pewno **fantomy** z chat-widgetu Chatwoot lub formularza `contact` (patrz sekcja o bugu GTM z 08.09.2026) — server-side licznik `/api/lead-conversion.ts`, czyli jedyne wiarygodne źródło prawdy, **nigdy nie zaliczył ani jednej konwersji Xenia**. Zwróć uwagę na kierunek metryk: CTR wzrósł z 4,94% do 11,71%, a CPC spadł z €1,90 do €0,91 — czyli optymalizacje z 08.09 (negatywy, geo) **zadziałały na poziomie ruchu**, ale ruch nadal nie zamieniał się w leady. To argument, że problem leży po stronie dopasowania oferty/rynku albo landing page, nie po stronie ustawień kampanii.
- **Remarketing — stan na 14.09.2026 (Audience Manager):** najlepsza lista **"All visitors (Google Ads)" ma tylko 64 użytkowników** (Search 64 / YouTube 64 / Display 40 / Gmail 16). Wszystkie listy na koncie mają status **"Too small to serve"** — do progu ~100 aktywnych userów w 30 dni, wymaganego przez Google do serwowania Display/PMax, wciąż daleko. Pozostałe listy: "All Users of marotino.com" 0, "Purchasers of marotino.com" 0, "All converters" — Display 8, reszta "Populating...". **Dwa wnioski:** (1) decyzja z 30.08 o odłożeniu banerów/PMax do czasu zebrania listy była słuszna — gdybyśmy uruchomili Display wcześniej, nie miałby do kogo trafiać; (2) po zapauzowaniu kampanii Search ta lista **przestanie rosnąć**, więc remarketing dla Xenii jest zamrożony do czasu ewentualnego wznowienia.
- **08.09.2026:** naprawiony GTM trigger, który mieszał konwersje "Xenia Lead Submitted" z chat-widgetem i formularzem contact (patrz sekcja niżej) — GTM Version 5, live.
- **04.09.2026:** formularz `xenia-pilot` uproszczony z 6 pól (property, email, rooms, tier, message + honeypot) do 4 wymaganych pól (imię, email, telefon, nazwa hotelu) — usunięte pole `message`. Deploy `78f91fb`, zweryfikowany na żywo 08.09.2026: formularz i GTM działają bez regresji.
- **08.09.2026:** dodane 4 negatywne słowa kluczowe na poziomie kampanii (exact match): `best ai for business`, `conversational ai platform`, `ai service`, `best ai platforms for business` — generyczne zapytania o "AI dla biznesu" bez intencji hotelarskiej, 0 konwersji, ~€21 skonsumowanego budżetu w tygodniu 1-7.09. Celowo zostawione `ai for customers` i `ai business` (po 1 konwersji każde, mimo niskiej relewancji) do obserwacji — zbyt mało danych żeby wykluczyć czy to prawdziwe konwersje.
- **08.09.2026:** odkryte i naprawione — jedyne zgłoszenie `xenia-pilot` od deployu nowego formularza (7.09, `imranshakil77@gmail.com`) miało **puste pola name i phone**, mimo że są `required` na żywej stronie. Przyczyna: request POSTowany bezpośrednio na endpoint Netlify Forms omija zarówno walidację `required` w przeglądarce, jak i honeypot — Netlify i tak zapisuje to jako "prawdziwe" zgłoszenie. `lead-conversion.ts` fanoutował to dalej jako pełnoprawną konwersję Google Ads/GA4 bez żadnej weryfikacji kompletności danych. Naprawa: endpoint teraz odrzuca (nie liczy jako konwersję) zgłoszenia bez niepustych `name`+`phone`+`email`. Commit `1cd1a09`, deployed.
- **Nowy wymóg Google (08.09.2026):** "Verify your identity" (weryfikacja reklamodawcy) do **2026-10-07**, inaczej część reklam może zostać wstrzymana/ograniczona. Osobny proces od developer tokena — wymaga ręcznej weryfikacji w Google Ads (Admin → dokumenty/pytania o firmę), nie da się zautomatyzować.

---

## Dlaczego Search, nie Performance Max

Kreator Google Ads domyślnie proponuje Performance Max ("Recommended") — to wymaga assetów graficznych (banery, obrazy) które w momencie startu nie istniały. **Search-only nie wymaga żadnych grafik** (Responsive Search Ads = czysty tekst), więc to był świadomy wybór, nie ograniczenie. Jeśli/kiedy powstaną banery, Display lub PMax można dodać jako osobną kampanię równoległą — nie mieszać w tej samej.

W ustawieniach kampanii **Google Display Network jest odznaczone** (domyślnie Google chce je włączyć nawet w kampanii Search — "recommended" — celowo wyłączone, bo bez grafik traci sens).

## Dlaczego Floryda, nie cała US ani Polska

Ogólna strona marotino.com pokazywała najwięcej ruchu z Polski w GA4, ale to ruch direct/branded (ludzie już znający firmę), nie realne poszukiwanie software house'u. Xenia sprzedaje się hotelom — Floryda/Miami pasuje do pozycjonowania Marotino jako "Miami-based software house" i do realnego rynku hotelarskiego. To był strzał na start bez danych segmentowanych per-produkt (GA4 nie rozróżnia ruchu na `/products/xenia` osobno w obecnym raportowaniu) — do zweryfikowania po pierwszych tygodniach danych.

## Struktura: keywords, ad copy

Kreator Google Ads **domyślnie zaproponował konsumenckie frazy hotelowe** ("hotel room", "nearby hotels", "discount hotels" — ludzie szukający noclegu) — kompletnie złe dla B2B softu sprzedawanego hotelarzom. Podmienione na 16 fraz B2B:

```
ai hotel receptionist, ai concierge for hotels, hotel chatbot software,
white label hotel app, branded hotel mobile app, hotel guest messaging software,
hotel whatsapp automation, hotel virtual assistant software, conversational ai hospitality,
ai front desk hotel, hotel guest communication platform, ai hotel concierge software,
automated hotel guest messaging, hotel management chatbot software, hotel ai software solution,
hotel chatbot software
```

Podobnie **AI-generowane nagłówki reklam były bez sensu** ("Without Your Own Channel", "Same Facts, Any Language", "You Have Twenty Minutes" — Ad strength "Poor"). Podmienione na treści wyciągnięte z realnej strony Xenia (live in 6 weeks, no setup fee, 40+ languages, 30-day risk-free pilot, white-label). Po zmianie: Ad strength "Average", optimization score 92.5%+.

**4 sitelinki** dodane z unikalnymi kotwicami na stronie (żeby uniknąć warningu "duplicate sitelink" — Google wymaga unikalnego final URL per sitelink):
- Talk to Sales → `/products/xenia#pilot`
- License Spec → `/products/xenia#license`
- How It Works → `/products/xenia#platform`
- About Marotino → `/about`

## Konwersje — jak to działa

Formularz na stronie: `xenia-pilot` (Netlify Forms), pola property/email/message + honeypot. **Ważna pułapka odkryta podczas testów:** formularz wysyła realny submit przez JS `fetch()` na **`/netlify-forms.html`** (osobna statyczna strona-wabik do wykrycia przez bota Netlify przy buildzie), **NIE** na adres samej strony `/products/xenia`. Test POST na zły URL (adres strony) zwraca 200 ale **nie rejestruje leada** — trzeba było posłać na `/netlify-forms.html`, dopiero wtedy przyszła strona "Thank you!" i mail z powiadomieniem. Jeśli kiedyś trzeba znowu przetestować formularz ręcznie (curl), pamiętać o tym endpoincie.

Google Ads ma dwie conversion actions:
1. **"Lead form - Submit"** (Google hosted, Primary) — utworzona automatycznie przez kreator kampanii po wybraniu celu "Submit lead form".
2. **"Form"** (Website, Primary) — auto-detekcja przez współdzielony tag Google (gtag.js, ten sam co GA4 `G-JT28P4C0LD`). Status na starcie: **"Unverified" / "Inactive"** — to normalne, weryfikacja zajmuje do 3h po pierwszej wizycie na stronie z tagiem, nie trzeba nic ręcznie budować w GTM.

**Nie trzeba budować osobnego taga konwersji w GTM** — Google Ads korzysta z już istniejącego gtag.js na stronie (ten sam załadowany przez GTM-5JRBQF9N dla GA4). Weryfikacja jest automatyczna.

## GTM / GA4 — zweryfikowane działające (30.08.2026)

- GTM kontener: **GTM-5JRBQF9N** (konto "Marotino CY LTD"), Consent Mode v2 poprawnie ustawiony przed załadowaniem GTM.
- GA4: **G-JT28P4C0LD**, potwierdzone żywe trafienia `page_view`/`user_engagement` do `region1.google-analytics.com` (HTTP 204).
- Property GA4 połączone z kontem Ads podczas setupu kampanii (marotino.com, property `531284463`).

## Display/PMax assety graficzne — zatwierdzone (30.08.2026)

Trzy formaty gotowe w `assets/approved/` (landscape 1200×628, square 1200×1200, portrait 960×1200) — zaakceptowane przez Cezarego. Wygenerowane w Nano Banana Pro / ChatGPT image gen (hotel lobby + telefon z chatem AI, bez logo — model źle renderuje litery, więc logo/wordmark dodawane osobno w PIL), nagłówki nałożone osobno (Python/Pillow, font Arial Bold + gradient scrim). Podgląd był publikowany jako Artifact do akceptacji wizualnej.

**Pułapka przy nakładaniu tekstu:** licz szerokość nagłówka względem dostępnej szerokości przed wpisaniem na sztywno — w square format nagłówek "24/7 AI Concierge" nachodził na telefon przy pierwszej wersji (za szeroki na zarezerwowaną strefę). Poprawka: zawijanie tekstu mierzone realną szerokością pixelową (`draw.textbbox`), nie liczbą znaków — przy różnych proporcjach kadru (square vs portrait vs landscape) ta sama liczba znaków ma inną szerokość względem dostępnego miejsca.

**Nie wgrane jeszcze do kampanii.** Plan: uruchomić jako kampanię Display/PMax **dopiero gdy lista remarketingowa (GA4 → Ads, odwiedzający `/products/xenia` z kampanii Search) osiągnie sensowny rozmiar** (Google wymaga ~100 aktywnych userów w 30 dni do serwowania) — świeży ruch z Display na zimno konwertuje słabo dla niszowego B2B, remarketing na ludzi którzy już widzieli stronę ma dużo lepsze szanse. Sprawdzić rozmiar audiencji w **Tools & Settings → Audience Manager**.

**Brakuje jeszcze:** logo (1200×1200 kwadrat + 1200×300 poziome) — do zrobienia czystym tekstem w PIL, nie przez model graficzny (ryzyko glitchu w renderowaniu liter).

## Pułapki podczas zakładania konta (żeby nie powtórzyć)

1. **Konto agencyjne z wieloma klientami pod jednym loginem** (ostrowski@marotino.com) — na liście kont Ads są zawieszone/w budowie drafty innych klientów (Optienergia, Batycki, Tincors, Menusso, itd.). Kreator "Create your first campaign" **domyślnie podłącza się do ostatniego niedokończonego draftu** — jeden z nich (konto 838-841-9692) miał wpisane dane **Optienergia** zamiast Marotino. Zawsze sprawdzić przy starcie nowej kampanii, czy nie kontynuujemy cudzego draftu — użyć przycisku "New Google Ads Account" → "Create a new account", nie "Finish setting up".
2. **Waluta konta domyślnie ustawiła się na PLN** mimo że Marotino CY LTD rozlicza się w EUR — trzeba było ręcznie zmienić w kroku budżetu (dropdown przy polu kwoty). **Kraj konta ("Germany" domyślnie, źle wykryty) też trzeba było poprawić na Cyprus** — dopiero po tej poprawce Google Ads pokazał już istniejący profil płatności Marotino CY LTD (wcześniej sugerował założenie nowego z danymi osobistymi z konta Google).
3. **Profil płatności Marotino CY LTD JUŻ ISTNIEJE** (ID: 0708-1259-6138, współdzielony z innymi usługami Google Ads/Cloud) — nie trzeba zakładać nowego. Dane: VAT CY60017620T, adres Evripidou 9A, 3031 Limassol, Cyprus.
4. **Pierwsza autoryzacja karty (Visa …9422) nie powiodła się** przy pierwszej próbie (bank oznaczył jako podejrzaną — typowe przy pierwszej płatności zagranicznej/online). Zadziałało po odblokowaniu karty przez użytkownika i dodaniu jej ponownie.
5. Krok płatności wymaga **"Verify it's you"** — weryfikacja tożsamości w osobnym oknie popup, którą musi przejść człowiek (2FA), nie da się zautomatyzować.

## Pułapka: karta 9422 przestała być obciążana (31.08.2026)

Dzień po starcie kampanii pojawił się czerwony błąd **"Payment method can't be charged"** w diagnostyce kampanii — **0 impresji od 30.08 mimo statusu Enabled**. Primary payment method (Visa •••• 9422, ta sama karta co miała problem z autoryzacją przy zakładaniu konta) przestała być obciążana. Ręczna wpłata €100 inną kartą (Visa •••• 6042, "Make an optional payment") zbilansowała konto (Balance €0.00) ale **nie naprawiła problemu** — primary payment method nadal była 9422 i błąd nie znikał, kampania dalej nie serwowała reklam. Dopiero po naprawieniu/zmianie primary payment method błąd zniknął (status zmienił się z czerwonego "misconfigured" na łagodniejsze żółte "only eligible to serve to a limited audience" — to już nie blokuje serwowania, tylko normalny etap dla nowej kampanii).

**Wniosek:** przy problemach z płatnością nie wystarczy dopłacić ręcznie z innej karty — trzeba naprawić/zmienić samą **primary payment method** w Billing → Settings → Payment methods, inaczej Google Ads dalej traktuje kampanię jako niesprawną. Warto też dodać **backup payment method** (na dziś: brak — osobny warning w Billing Settings), żeby przyszłe awarie karty głównej nie zatrzymywały kampanii.

## Pułapka: zmiana slugu strony złamała final URL-e (31.08.2026)

Po naprawieniu płatności diagnostyka nadal pokazywała czerwony błąd **"Your website is missing a Google tag"** mimo że tag GTM-5JRBQF9N faktycznie ładuje się na stronie. Przyczyna: **slug strony produktowej zmienił się** z `/products/xenia` na `/products/xenia-white-label-mobile-app-rag` już po ustawieniu kampanii (301 redirect ze starego na nowy) — final URL reklamy i 3 sitelinki (Talk to Sales, License Spec, How It Works) nadal wskazywały na stary, przekierowujący adres. Google Ads nie wykrywa taga niezawodnie przez redirect, więc kampania dalej nie serwowała mimo statusu "Enabled".

**Wniosek:** jeśli strona produktowa dostanie nowy slug (np. przy pracach SEO/i18n), trzeba ręcznie zaktualizować final URL + sitelinki w Google Ads — redirect 301 nie wystarcza, Ads traktuje to jako brak taga. Sprawdzać final URL-e przy każdej zmianie struktury URL na stronie, nie tylko przy starcie kampanii.

**Naprawa:** final URL i sitelinki `#pilot`/`#license`/`#platform` podmienione na `/products/xenia-white-label-mobile-app-rag` (anchory istnieją na nowej stronie, zweryfikowane).

## Prawdziwa przyczyna "missing Google tag" — brakował dedykowany tag Ads (31.08.2026)

Po naprawie redirecta błąd "Your website is missing a Google tag" **dalej się utrzymywał** w diagnostyce (0 impressions). Kliknięcie "Fix it" w Campaign diagnostics pokazało konkretny Tag ID: **AW-18418762437** — dedykowany tag konwersji Google Ads, osobny od GA4 (`G-JT28P4C0LD`). Wcześniejsze założenie w tym README ("Google Ads korzysta z już istniejącego gtag.js dla GA4, nie trzeba nic budować w GTM") było **błędne** — Google Ads chce zobaczyć swój własny tag AW- na stronie, GA4 shared tagging to nie zastępuje.

**Naprawa:** w GTM-5JRBQF9N (konto Marotino CY LTD, kontener marotino.com) dodany nowy tag typu **Google Tag** z ID `AW-18418762437`, trigger "Initialization - All Pages", opublikowany jako **Version 3** (31.08.2026, 21:12). Zweryfikowane, że GTM nadal ładuje się na `/products/xenia-white-label-mobile-app-rag`.

**Wniosek na przyszłość:** przy kampanii Google Ads z celem "Submit lead form" / conversion tracking na stronie **zawsze dodać dedykowany tag AW-XXXXXXXXX w GTM** (Tag type: "Google Tag"), nie polegać na tym że wspólny tag GA4 wystarczy — Google Ads i GA4 to osobne ID mimo współdzielonego mechanizmu gtag.js. Diagnostyka Ads potrzebuje do ~3h żeby zweryfikować nowy tag po publikacji.

## Prawdziwy, ostateczny root cause: auto-wykrywany "Form" conversion action nigdy się nie zweryfikuje (01.09.2026)

Po dodaniu taga AW-18418762437 błąd "missing Google tag" **nadal się utrzymywał** po ponad 24h — zbyt długo jak na zwykłe opóźnienie weryfikacji (~3h). Sieciowo potwierdzone (DevTools network), że tag faktycznie strzela poprawnie na żywo (`gtag/js?id=AW-18418762437`, realny hit `google.com/ccm/collect?...tid=AW-18418762437`). Status kampanii w Google Ads to dosłownie **"Eligible (Misconfigured)"** — to aktywnie ogranicza serwowanie, nie tylko kosmetyczny warning.

**Prawdziwa przyczyna:** account-default primary conversion action **"Form"** (Website, auto-utworzona przez kreator kampanii 30.08) nasłuchuje na **natywne zdarzenie `submit` formularza** na `marotino.com/products/xenia` (Google's automatic form-detection). Ale strona **nie wysyła natywnego submitu** — JS w `xenia-white-label-mobile-app-rag.astro` robi `e.preventDefault()` i zamiast tego wysyła `fetch()` w tle na `/netlify-forms.html` (patrz pułapka wyżej). Automatyczna detekcja Google nigdy nie widzi natywnego submitu, więc ta konkretna akcja **nigdy się nie zweryfikuje, niezależnie ile się czeka** — to nie kwestia czasu, tylko strukturalna niezgodność.

**Naprawa (bez zmian w kodzie strony — kod już wysyłał właściwy sygnał):**
1. Kod strony **już** robił `window.dataLayer.push({event: 'generate_lead', form_name: 'xenia', ...})` po udanym `fetch()` (ten sam kontrakt co `/contact`, patrz `xenia-white-label-mobile-app-rag.astro` ~linia 4136). GTM ma już trigger `CE - generate_lead` podpięty pod GA4 od 10.08.2026 — działający, sprawdzony kanał.
2. W Google Ads utworzona nowa conversion action **"Xenia Lead Submitted (generate_lead)"** (Website, Manually with code, Primary), conversion label `AW-18418762437/9eaGCIL32-scEMWF4M5E`.
3. W GTM-5JRBQF9N dodany tag **"Google Ads Conversion Tracking"** (Conversion ID `18418762437` — bez prefixu "AW-" w tym polu, Conversion Label `9eaGCIL32-scEMWF4M5E`), fire na tym samym triggerze `CE - generate_lead`. Opublikowane jako **Version 4** (01.09.2026, 08:50).
4. Martwa akcja **"Form"** zdemotowana z Primary na **Secondary** (nadal widoczna w "All conversions", ale nie blokuje już celu "Submit lead forms").

**Wniosek na przyszłość:** jeśli formularz na stronie wysyła się przez JS `fetch()`/AJAX zamiast natywnego `<form>` submit (częste przy custom walidacji, Netlify Forms via AJAX, SPA), **auto-wykrywana "Website" conversion action Google Ads nigdy nie zadziała** — trzeba ręcznie stworzyć conversion action typu "Manually with code" i podpiąć pod istniejące zdarzenie sukcesu (dataLayer push / custom event), najlepiej reużywając trigger już używany przez GA4, nie duplikować logiki w kodzie strony. Objaw ("Eligible (Misconfigured)", 0 impressions mimo Enabled) wygląda identycznie jak zwykłe opóźnienie weryfikacji taga — rozróżnić można tylko sprawdzając czy strona w ogóle wysyła natywny submit (DevTools → Elements → sprawdzić czy jest `preventDefault()` na formularzu).

## Prawdziwy, PRAWDZIWY ostateczny blocker: niewypełniony formularz zgodności UE (01.09.2026)

Po wszystkich powyższych naprawach (tag, conversion action) kampania **nadal** miała 0 impressions — ponad 3 dni od startu. Wszystkie elementy (keywords, ad, ad group, billing) pokazywały "Eligible" pojedynczo, ale kampania jako całość nie serwowała ani jednej reklamy.

**Prawdziwa przyczyna:** w **Admin → Policy → Account** konto miało niewypełnione, obowiązkowe pytanie regulacyjne: *"Plan to run European Union political ads?"* — wymagane przez prawo UE, bo profil płatności to Marotino CY LTD (Cypr = UE). To nie było opcjonalne zadanie "verification" (te są faktycznie opcjonalne) — to osobna, obowiązkowa deklaracja, która najwyraźniej blokowała serwowanie całej kampanii dopóki nie została odpowiedziana.

**Naprawa:** Admin → Policy → Account → odpowiedziane "No, I don't plan to use this account to run EU political ads". Natychmiast po tym (w ciągu godziny) kampania zaczęła serwować: pierwszego dnia 18 impressions, 2 clicks, €7.72 kosztu.

**Wniosek na przyszłość:** jeśli konto Google Ads ma profil płatności zarejestrowany w UE (nawet gdy kampania celuje poza UE, jak tu Floryda), **sprawdzić Admin → Policy → Account przy starcie każdej nowej kampanii** — nieodpowiedziane pytanie o EU political ads może cicho blokować serwowanie bez żadnego wyraźnego komunikatu w diagnostyce kampanii. To ukryty, osobny od reszty diagnostyki mechanizm.

## Fantomowe konwersje i przejście na server-side tracking (05.09.2026)

**Objaw:** w GA4/Google Ads pojawiła się konwersja `generate_lead`, ale w Netlify Forms (`xenia-pilot`) **nie było żadnego odpowiadającego submission** (ani verified, ani spam) w tym samym oknie czasowym. Sesja miała `First user source/medium = (not set)` z ikoną ostrzeżenia i kanał `Unassigned` w GA4 explore — typowy odcisk bota, nie realnego użytkownika.

**Prawdziwa przyczyna:** klient (JS w `xenia-white-label-mobile-app-rag.astro`) strzelał `dataLayer.push({event: 'generate_lead', ...})` na podstawie samego `if (res.ok)` z AJAX POST-a do `/netlify-forms.html`. **2xx z tego fetcha potwierdza tylko, że request nie zwrócił błędu sieciowego — nie potwierdza, że Netlify faktycznie zapisał submission.** Netlify po cichu odrzuca spam/honeypot submissions, ale wciąż odpowiada 200. Bot, który POST-uje bezpośrednio na ten endpoint (pomijając realny formularz i jego walidację/honeypot), mógł więc wywołać "sukces" po stronie JS bez zostawienia śladu w Netlify Forms.

**Naprawa — przeniesienie całej logiki konwersji na serwer:**
1. Usunięty client-side `dataLayer.push('generate_lead')` po `res.ok`. Formularz nadal wysyła się tak samo (AJAX na `/netlify-forms.html`), ale **nie odpala już żadnej konwersji sam z siebie**.
2. Dodane ukryte pola `gclid` i `ga_client_id` do formularza `xenia-pilot` (czytane z URL / cookie `_gcl_aw` i cookie `_ga`), wypełniane tuż przed każdym submitem (nie tylko raz przy załadowaniu strony — `form.reset()` po sukcesie czyściłby je przy drugim submicie tej samej sesji).
3. Nowy endpoint **`src/pages/api/lead-conversion.ts`** (Astro API route, deployowany jako część funkcji "Astro SSR" na Netlify) — jedyne miejsce, które faktycznie odpala konwersję. Chroniony shared-secretem w query stringu (`?key=...`, zmienna `LEAD_WEBHOOK_SECRET`).
4. Skonfigurowany **prawdziwy Netlify Forms outgoing webhook** (Site configuration → Notifications → Form submission notifications, event "New form submission", form **`xenia-pilot`**) wskazujący na `https://marotino.com/api/lead-conversion?key=<LEAD_WEBHOOK_SECRET>`. To jedyne wywołanie, które Netlify robi **dopiero po** zweryfikowaniu i zapisaniu submission — więc jest to prawdziwe źródło prawdy "lead się faktycznie wydarzył", którego zabrakło w starym mechanizmie.
5. Endpoint fan-outuje zweryfikowany event do dwóch miejsc:
   - **GA4 Measurement Protocol** (`GA4_MEASUREMENT_ID` / `GA4_API_SECRET`) — działa od razu, niezależne od Google Ads.
   - **Google Ads `uploadClickConversions`** (offline click conversion import, `gclid` + conversion action `7742094210` na koncie 846-403-8850) — wymaga OAuth (refresh token) i developer tokena z **Basic Access** (patrz niżej).

**Zmienne środowiskowe ustawione w Netlify (Site configuration → Environment variables, All scopes / Same value for all deploy contexts):** `LEAD_WEBHOOK_SECRET`, `GA4_MEASUREMENT_ID`, `GA4_API_SECRET`, `GOOGLE_ADS_DEVELOPER_TOKEN`, `GOOGLE_ADS_LOGIN_CUSTOMER_ID` (MCC 331-108-1372), `GOOGLE_ADS_CUSTOMER_ID` (846-403-8850 bez myślników → `8464038850`), `GOOGLE_ADS_CONVERSION_ACTION_ID` (`7742094210`), `GOOGLE_ADS_CLIENT_ID`, `GOOGLE_ADS_CLIENT_SECRET`, `GOOGLE_ADS_REFRESH_TOKEN` (OAuth client + refresh token wygenerowane przez Google Cloud project pod `menusso-402408`, autoryzowane przez MCC 331-108-1372 → konto 846-403-8850; refresh token zdobyty ręcznie przez OAuth 2.0 Playground, bo Google wymagał utworzenia passkey przy autoryzacji — krok, którego nie da się zautomatyzować).

**Pułapka po drodze:** przy pierwszym zapisie `GOOGLE_ADS_REFRESH_TOKEN` w UI Netlify wartość **nie zapisała się** (formularz pokazywał "0 values in all deploy contexts" mimo widocznego "Create variable") — trzeba było usunąć i dodać ponownie, wpisując wartość przez kliknięcie w pole + wpisanie znak-po-znaku zamiast jednorazowego "fill" całej wartości na raz (coś w formularzu Netlify nie rejestrowało programatycznego ustawienia value bez realnych zdarzeń klawiatury). **Wniosek na przyszłość:** po każdym zapisie sekretu w Netlify UI zweryfikować, że pole faktycznie pokazuje "X value(s) in Y deploy context(s)", nie zakładać sukcesu tylko po komunikacie "Create variable" bez błędu.

**Aktualny blocker (poza naszą kontrolą):** developer token Google Ads ma poziom **"Explorer Access"** — pozwala wywoływać API tylko na testowych/sandboxowych kontach, nie na prawdziwe konto 846-403-8850. Złożony wniosek o **"Basic Access"** (formularz Google, 04.09.2026), odpowiedź w ciągu ~5 dni roboczych. Dopóki nie przyjdzie zgoda, `uploadClickConversions` będzie zwracać błąd autoryzacji przy każdej próbie — kod to łapie (`try/catch`) i loguje, **nie crashuje** endpointu ani nie blokuje reszty fan-outu (GA4 nadal dostaje event). Po przyznaniu Basic Access nie trzeba nic zmieniać w kodzie — sam upload zacznie przechodzić.

**Zweryfikowane działanie endpointu na żywo (05.09.2026, po deployu commit `039070c`):**
- brak/zły `?key=` → `403 Forbidden`.
- submission z formularza `contact` (nie `xenia-pilot`) → `200 OK`, treść `"OK (ignored form)"` — filtr po `form_name` działa.
- submission `xenia-pilot` bez `gclid`/`ga_client_id` w danych → `200 OK`, nie crashuje (graceful skip zamiast wywalenia się).

## GTM trigger mieszał konwersje Xenia z chat-widgetem i formularzem contact (08.09.2026)

**Objaw:** mimo że w piątkowym fixie (05.09.2026, patrz sekcja wyżej) usunąłem client-side `dataLayer.push('generate_lead')` z formularza `xenia-pilot`, konwersja Google Ads **"Xenia Lead Submitted (generate_lead)"** dalej zbierała nowe konwersje (3 sztuki w oknie 6-8.09.2026) — mimo że formularz Xenia fizycznie już nic nie wysyła do dataLayer.

**Przyczyna:** GTM trigger **`CE - generate_lead`** (podpięty pod ten conversion tag od 01.09.2026, patrz sekcja "Prawdziwy, ostateczny root cause" wyżej) nasłuchuje na **zdarzenie `generate_lead` bez żadnego filtra po `form_name`** ("This trigger fires on: All Custom Events"). Ten sam generyczny event `generate_lead` jest też wysyłany przez:
- `src/pages/contact.astro` (formularz kontaktowy, `form_name: 'contact'`) — osobny formularz, inny cel.
- `src/layouts/BaseLayout.astro` (widget czatu Chatwoot, `form_name: 'chat_widget'`) — fire raz na sesję przy **otwarciu** czatu, **na dowolnej stronie serwisu**, nie tylko `/products/xenia`.

Efekt: konwersja "Xenia Lead Submitted" liczyła (i po fixie z 05.09 liczy **wyłącznie**) otwarcia czatu i submity formularza `contact` — czyli osoby, które kliknęły reklamę Xenia, ale nigdy nie wypełniły pilotażowego formularza. To był problem pomiarowy od 01.09.2026 (kiedy podpięto Ads-conversion pod ten sam współdzielony trigger co GA4), niezauważony bo wcześniej maskowany prawdziwym sygnałem z formularza Xenia.

**Naprawa:** w GTM-5JRBQF9N (kontener marotino.com) dodany nowy trigger **`CE - generate_lead (xenia)`** — Custom Event `generate_lead`, warunek `{{DLV - form_name}} equals xenia` (zmienna `DLV - form_name` już istniała, użyta wcześniej przy budowie oryginalnego triggera). Tag **"Google Ads Conversion - Xenia Lead Submitted"** przepięty z `CE - generate_lead` na nowy, scoped trigger. Opublikowane jako **Version 5** (08.09.2026, 11:44).

**Ważna świadoma konsekwencja:** ponieważ formularz Xenia **celowo** już nie wysyła client-side `dataLayer.push` (to był fix z 05.09, patrz wyżej — optimistic client push to źródło fantomowych konwersji), nowy trigger `CE - generate_lead (xenia)` **obecnie nic nie odpala** — nikt nie publikuje zdarzenia z `form_name: 'xenia'` po stronie klienta. To jest zamierzone i poprawne: prawdziwe śledzenie konwersji Xenia dla Google Ads żyje teraz **wyłącznie server-side** w `/api/lead-conversion.ts` (`uploadClickConversions`, triggerowane przez zweryfikowany webhook Netlify Forms), nie w GTM. GTM-owy tor jest zachowany jako czysty/scoped na wypadek gdyby ktoś w przyszłości chciał dodać dodatkowy client-side sygnał, ale nie jest to obecny plan.

**Wniosek na przyszłość:** przy podpinaniu nowej Google Ads conversion action pod istniejący GTM trigger — zawsze sprawdzić czy trigger jest scoped do konkretnego `form_name`/eventu, czy łapie generyczne zdarzenie współdzielone przez wiele formularzy/widgetów na stronie. Reużywanie tego samego nazwanego eventu (`generate_lead`) dla wielu niepowiązanych źródeł leadów (kontakt, czat, produkt) jest wygodne dla GA4 (jeden "key event"), ale niebezpieczne dla per-kampanijnych conversion actions w Google Ads, jeśli trigger nie filtruje po `form_name`.

## Plan poprawy konwersji (08.09.2026)

Punkt wyjścia: tydzień 1-7.09 — 80 kliknięć, €151.99, konw. rate 2.50% wg Ads (dane niepewne, patrz sekcja o fantomowych konwersjach — realny licznik to server-side `/api/lead-conversion.ts`, wciąż czeka na Basic Access). Priorytety w kolejności wykonania:

1. **Zrobione — formularz.** 6 pól → 4 pola (imię, email, telefon, hotel). Mniej tarcia = więcej ukończonych submitów przy tym samym ruchu. To był największy pojedynczy dźwigień, bo to jedyny etap lejka w pełni pod naszą kontrolą.
2. **Zrobione — negatywy.** Odcięte 4 najgorsze frazy generyczne ("ai platform/service/for business" bez kontekstu hotelarskiego) — ~21€/tydzień oszczędności, realokowane na relewantne kliknięcia.
3. **Zrobione — geo.** Floryda i Nowy Jork usunięte jako osobne lokalizacje (były podzbiorem "East Coast of the United States", zero utraty zasięgu, czystszy reporting).
4. **Do zrobienia — dopasowanie przekazu reklama↔strona.** Reklamy mówią o "AI recepcjoniście" ogólnie; sprawdzić czy nagłówki RSA i landing page (sekcja hero) używają tych samych słów co najlepiej konwertujące frazy (`prohost ai`, `online reservation system`, `resort management`) — Google nagradza spójność query↔ad↔landing wyższym Quality Score i niższym CPC.
5. ~~**Blocker poza naszą kontrolą — Basic Access developer tokena.**~~ **NIEAKTUALNE od 14.09.2026** — patrz sekcja "Developer token przestał być blockerem" niżej. Google zlikwidował wymóg developer tokena; czekanie na maila z Basic Access było bezprzedmiotowe.
6. **Do zrobienia po punkcie 5 — bid strategy.** Wrócić z "Maximize clicks" na "Maximize conversions" dopiero gdy server-side conversion (`Xenia Lead Submitted`, action `7742094210`) ma realne dane napływające z `uploadClickConversions` — inaczej strategia znowu zdusi serwowanie (patrz incydent 31.08).
7. **Odłożone — remarketing/Display.** Assety graficzne gotowe, ale lista remarketingowa wymaga ~100 aktywnych userów w 30 dni (Audience Manager) — przy 80 kliknięciach/tydzień to kilka tygodni. Nie uruchamiać przedwcześnie, zimny ruch na Display dla niszowego B2B konwertuje słabo.

## Developer token przestał być blockerem (14.09.2026)

Od 04.09.2026 cała ścieżka server-side (`uploadClickConversions` w `/api/lead-conversion.ts`) czekała na przyznanie **Basic Access** dla developer tokena — wniosek złożony, Google deklarował ~5 dni roboczych, termin mijał ok. 11.09. W README figurowało to jako "blocker poza naszą kontrolą" i priorytet #1.

**Okazało się, że czekamy na coś, co przestało istnieć.** W Google Ads API Center (konto managera **331-108-1372**, Admin → API center) widnieje teraz komunikat:

> **Google Ads API access has changed**
> - Developer tokens are no longer required for using the Google Ads API.
> - API access levels are now managed exclusively in the Google Cloud Console. The levels displayed on this page may no longer be accurate and cannot be upgraded from this page.
> - You can still update your API contact email here, which will be used for developer outreach and important updates.

Strona **nadal pokazuje "Access level: Explorer Access"** — i to jest właśnie pułapka: ta etykieta, według samego Google, **może już nie odpowiadać rzeczywistości** i nie da się jej z tego miejsca podnieść. Czyli status widoczny w API Center przestał być wiarygodnym źródłem prawdy o tym, czy API zadziała.

**Wniosek na przyszłość:** nie diagnozować dostępu do Google Ads API po etykiecie w API Center i nie czekać na maila z decyzją o Basic Access. Zamiast tego **po prostu zawołać `uploadClickConversions` i zobaczyć, czy przechodzi** — kod w `/api/lead-conversion.ts` i tak łapie błąd w `try/catch` i loguje go do Netlify function logs, więc próba nic nie kosztuje i nie wywraca endpointu. Jeśli wywołanie zwróci błąd autoryzacji, poziomów dostępu szukać w **Google Cloud Console** (projekt `menusso-402408`, pod którym wygenerowano OAuth client), a nie w Google Ads.

**Uwaga przy okazji:** kod ma zaszytą wersję API `v18` (`GOOGLE_ADS_API_VERSION || 'v18'`) z komentarzem, żeby zweryfikować ją przed pójściem na żywo — Google wycofuje wersje mniej więcej co rok. Przy pierwszej realnej próbie uploadu sprawdzić, czy `v18` jeszcze żyje, bo błąd nieistniejącej wersji API będzie wyglądał myląco podobnie do błędu autoryzacji.

## Do zrobienia / do obserwowania

- [x] ~~Sprawdzić za kilka godzin czy status kampanii zmienił się z "Eligible (Misconfigured)" na normalny "Eligible" i czy zaczęły się impressions.~~ **Rozwiązane 01.09.2026** — patrz wyżej.
- [ ] Po pierwszym tygodniu: sprawdzić czy geo Floryda faktycznie łapie relewantny ruch, czy trzeba rozszerzyć/zawęzić.
- [ ] Rozważyć banery/PMax jako kampanię równoległą, jeśli powstaną assety graficzne.
- [ ] Zdecydować docelowy budżet dzienny po zobaczeniu realnego CPC/CPA z pierwszego tygodnia (start: €20/dzień to smoke test, nie budżet docelowy).
- [ ] Ustawić alert/przegląd tygodniowy leadów z formularza `xenia-pilot` (Netlify Forms dashboard) vs conversions w Google Ads — porównać czy się zgadzają.
- [ ] Po zebraniu pierwszych realnych konwersji z "Xenia Lead Submitted": wrócić z bid strategy na "Maximize conversions" (patrz incydent 31.08.2026 wyżej).
- [ ] Rozważyć usunięcie/wyłączenie martwej akcji "Form" (Secondary) po potwierdzeniu że nowa akcja działa — żeby nie zaśmiecać listy conversion actions.
- [x] ~~Sprawdzić maila / status wniosku o **Basic Access** developer tokena (złożony 04.09.2026, ~5 dni roboczych).~~ **Bezprzedmiotowe od 14.09.2026** — Google zlikwidował wymóg developer tokena, patrz sekcja "Developer token przestał być blockerem".
- [ ] **Zamiast czekania:** zawołać `uploadClickConversions` na żywo i sprawdzić w Netlify function logs, czy przechodzi (najprościej: realny submit formularza z `gclid` w URL, albo ręczne wywołanie endpointu). Przy okazji zweryfikować, czy wersja API `v18` w `/api/lead-conversion.ts` jeszcze żyje.
- [ ] Jeśli upload przechodzi: zweryfikować że conversion action `7742094210` faktycznie zbiera dane w Google Ads (Conversions → Xenia Lead Submitted) i porównać liczbę z Netlify Forms dashboard dla `xenia-pilot`. **Uwaga:** kampania Xenia jest zapauzowana od 14.09, więc bez wznowienia nie będzie nowych kliknięć z `gclid` do przetestowania — testować na kampanii lead-gen Marotino.
- [ ] Zrobić **advertiser verification** w Google Ads (Admin → Policy) przed **2026-10-07**, inaczej część reklam może zostać wstrzymana/ograniczona. Wymaga ręcznego działania (dokumenty/pytania o firmę), nie da się zautomatyzować. **To dotyczy całego konta, więc blokuje też nową kampanię lead-gen** — termin się nie odsunął przez zapauzowanie Xenii.
- [ ] Po zebraniu pierwszych realnych server-side konwersji: rozważyć, czy warto reaktywować client-side sygnał GTM (`CE - generate_lead (xenia)` — obecnie nic go nie publikuje) jako dodatkowe źródło, czy zostać wyłącznie przy server-side jako jedynym źródle prawdy.
- [ ] Zdecydować, co dalej z kampanią Xenia: wznowić z poprawionym przekazem/landingiem, czy zostawić zapauzowaną i skupić budżet na lead-genie Marotino. Przy wznowieniu pamiętać, że lista remarketingowa (64 userów) w międzyczasie się starzeje — okno członkostwa leci dalej.

## Incydent 31.08.2026 — kampania 0 impressions od startu, naprawione

**Objaw:** Dzień po starcie (30.08) kampania miała **0 impressions / 0 clicks** w całej Florydzie mimo Enabled + budżetu. Diagnostyka Google Ads pokazywała ostrzeżenie **"Conversion tracking setup is incomplete"** i status kampanii **"Eligible (Limited)"**.

**Przyczyna:** Bid strategy to **"Maximize conversions"**, ale konto miało dwie sprzeczne primary conversion actions, obie bez danych:
1. **"Lead form - Submit"** (Google hosted, Primary) — widmo po kreatorze kampanii; kampania nie ma żadnego Lead Form asset (to Search-only z sitelinkami), więc ta akcja nigdy nie mogła nic zebrać.
2. **"Form" (Website, Primary)** — prawdziwe źródło (formularz `xenia-pilot` na stronie), ale status **"Inactive / Unverified conversion"** — Google nie widział jeszcze wizyty na stronie z tagiem po starcie kampanii.

Efekt: strategia "Maximize conversions" nie miała żadnego sygnału do optymalizacji, więc Google Ads świadomie ograniczał serwowanie reklam do ~zera. Błędne koło: brak impressions → brak kliknięć → brak wizyt na `/products/xenia` → tag nigdy się nie weryfikuje → tracking "incomplete" → kampania dalej ograniczona.

**Naprawa (wykonana bezpośrednio w panelu Google Ads):**
1. Ręcznie odwiedzona strona `/products/xenia`, żeby wywołać gtag.js (`G-JT28P4C0LD`) i przerwać błędne koło — potwierdzony żywy hit `page_view` (204) do `region1.analytics.google.com` z sygnałem `ads-audiences`. Weryfikacja "Form" powinna przejść w ciągu ~3h od tego triggera.
2. **"Lead form - Submit"** przełączone z Primary na **Secondary action** (Conversion actions → Action optimization) — nie blokuje już optymalizacji ani nie liczy się do account-default goal.
3. Bid strategy kampanii zmieniona z **"Maximize conversions"** na **"Maximize clicks"** — kampania może się wyświetlać bez czekania na dane konwersji. Do przywrócenia na "Maximize conversions", gdy zbiorą się pierwsze realne konwersje z formularza.

**Do zapamiętania na przyszłość:** kreator "Submit lead form" w Google Ads tworzy domyślnie Google-hosted Lead Form conversion action nawet gdy kampania nie ma żadnego Lead Form asset — trzeba to ręcznie zdezaktywować/przełączyć na Secondary przy starcie każdej nowej kampanii z tym celem, inaczej zafałszowuje sygnał optymalizacji obok prawdziwej konwersji ze strony.

---

## Kampania 2: Lead-gen Marotino (software house)

**Status: w budowie od 14.09.2026, jeszcze nie uruchomiona.**

Zupełnie inna kampania niż Xenia: nie promuje produktu, tylko **Marotino jako firmę** — software house sprzedający usługi (custom software, AI, mobile, SaaS MVP, team augmentation). Ta sama waluta i to samo konto Ads, ale inny odbiorca, inny landing i osobna akcja konwersji.

**Uwaga na geo:** pierwotnie kampania miała celować w Florydę (tak jak Xenia) i tak zapadła wstępna decyzja. Research słów kluczowych z tego samego dnia **odwrócił tę decyzję** — na Florydzie po prostu nie ma wystarczającego popytu. Kampania celuje w **całe USA** z korektą stawek na Miami. Szczegóły i liczby: sekcja „Research słów kluczowych (14.09.2026)" niżej.

Powód powstania: kampania Xenia po dwóch tygodniach i ~€284 wydatku nie dowiozła ani jednego realnego leada (patrz wyżej). Zamiast dalej dokładać do niszowego produktu B2B, budżet idzie na sprzedaż usług — czyli to, z czego Marotino faktycznie żyje i gdzie jest realne portfolio do pokazania.

## Decyzja: leady wyłącznie istniejącym formularzem z /contact

**Decyzja Cezarego z 14.09.2026:** leady zbieramy **wyłącznie istniejącym formularzem kontaktowym** na `marotino.com/contact`. Nie budujemy nowej dedykowanej landing page ani nowego formularza pod kampanię.

Konsekwencja, którą trzeba mieć z tyłu głowy — **ten formularz jest bardzo „luźny" jak na ruch płatny**. W `src/pages/contact.astro` wymagane jest **tylko pole `email`**; `name` i `message` są opcjonalne, a **pola telefon i nazwa firmy w ogóle nie istnieją**. Dla ruchu organicznego to sensowny wybór (minimum tarcia), ale przy ruchu płatnym oznacza to, że część zgłoszeń przyjdzie jako sam adres e-mail bez kontekstu — trudny do oddzwonienia i do zakwalifikowania.

Dla kontrastu: formularz `xenia-pilot` przeszedł 04.09 dokładnie odwrotną drogę — z 6 pól do 4 **wymaganych** (imię, email, telefon, hotel), właśnie po to, żeby lead dało się obsłużyć. Jeśli jakość leadów z `/contact` okaże się problemem, to jest pierwsze miejsce do zmiany (dodać wymagane imię + telefon), a nie budżet czy słowa kluczowe.

## Co zostało skonfigurowane 14.09.2026

### Nowa conversion action: "Marotino Lead - Contact Form"

W koncie 846-403-8850:

| Ustawienie | Wartość | Dlaczego |
|---|---|---|
| Kategoria | Submit lead form | zgodne z realnym celem |
| Źródło | Website, **Manually with code** | patrz niżej — auto-detekcja by nie zadziałała |
| Optymalizacja | **Primary** | ma sterować licytacją |
| Wartość | stała **€1** | wszystkie leady warte tyle samo; pozwala później przejść na Maximize conversions bez zgadywania wartości |
| Liczenie | **"One"** (nie "Every") | leady — liczy się pierwsza interakcja; „Every" zawyżałoby przy ponownym wysłaniu formularza |
| Click-through window | 90 dni | cykl decyzyjny w B2B software jest długi |
| Atrybucja | data-driven | domyślna, rekomendowana |
| **Conversion ID** | **18418762437** | ten sam tag AW co Xenia — jedno konto Ads, jeden tag bazowy |
| **Conversion Label** | **xLxkCPaHqPccEMWF4M5E** | unikalny dla tej akcji |

**Świadomie odznaczone "Conversions from phone calls"** w kreatorze. Kreator domyślnie zaznacza konwersje telefoniczne obok website'owych — to dokładnie ten sam mechanizm, który przy Xenii stworzył widmową akcję "Lead form - Submit" i rozmył sygnał optymalizacji (patrz incydent 31.08.2026). Jedna kampania = jeden czysty sygnał konwersji.

**Dlaczego "Manually with code", a nie auto-detekcja:** formularz `/contact` robi `e.preventDefault()` i wysyła dane przez `fetch()`, a nie natywnym submitem. Auto-wykrywana akcja typu Website nasłuchuje na **natywne zdarzenie `submit`** — więc **nigdy by się nie zweryfikowała**, niezależnie ile byśmy czekali. To jest ta sama strukturalna niezgodność, którą rozgryzaliśmy przy Xenii 01.09.2026 (sekcja "Prawdziwy, ostateczny root cause"); tym razem od razu poszliśmy właściwą ścieżką, bez traconych dni na „może tag się jeszcze zweryfikuje".

### GTM — trigger i tag (Version 6)

W kontenerze **GTM-5JRBQF9N** (konto Marotino CY LTD, marotino.com):

1. Nowy trigger **`CE - generate_lead (contact)`** — Custom Event `generate_lead`, warunek **`{{DLV - form_name}} equals contact`**. Zmienna `DLV - form_name` już istniała (użyta wcześniej przy triggerze dla Xenii).
2. Nowy tag **"Google Ads Conversion - Marotino Contact Lead"** — typ Google Ads Conversion Tracking, Conversion ID `18418762437` (bez prefiksu "AW-" w tym polu), Conversion Label `xLxkCPaHqPccEMWF4M5E`, fire na powyższym triggerze.
3. Opublikowane jako **Version 6** (14.09.2026, 10:23).

GTM przy zapisie taga potwierdził: *"Google tag found in this container — This tag will use the configuration of Google tag Marotino"*, czyli tag bazowy AW-18418762437 jest poprawnie wykrywany i nie trzeba go duplikować.

### Kluczowy kontekst: przed tą zmianą konto nie mierzyło NICZEGO

To jest najważniejsza rzecz do zrozumienia z tej sekcji. **Do 14.09.2026 konto Google Ads nie miało ani jednego działającego sygnału konwersji.** Złożyły się na to dwie rzeczy, każda sensowna z osobna:

- Jedyny tag konwersji Ads (`Google Ads Conversion - Xenia Lead Submitted`) wisiał na triggerze `CE - generate_lead (xenia)`, zawężonym 08.09 do `form_name = xenia`.
- Ale formularz Xenia **celowo przestał publikować cokolwiek do dataLayer** przy fixie z 05.09 (usunięcie optimistic client-side push jako źródła fantomowych konwersji).

Efekt: trigger poprawny, tag poprawny, tylko **nikt nigdy nie wysyła zdarzenia, które by go odpalił**. Było to świadome i opisane w sekcji z 08.09 („nowy trigger obecnie nic nie odpala… to jest zamierzone"), z założeniem, że prawdę zapewni ścieżka server-side — która z kolei stała zablokowana na developer tokenie (i, jak się okazało 14.09, czekała na coś nieistniejącego).

Formularz `/contact` przez cały ten czas **publikował** `generate_lead` z `form_name: 'contact'`, ale trafiało to wyłącznie do GA4 przez stary, niezawężony trigger `CE - generate_lead` — do Google Ads nie szło nic.

**Wniosek na przyszłość:** po każdej zmianie, która zawęża trigger albo usuwa client-side push, sprawdzić **całą ścieżkę end-to-end** (kto publikuje event → jaki trigger go łapie → jaki tag odpala → do jakiej akcji konwersji trafia), a nie tylko ten jeden element, który się zmienia. Dwie poprawne zmiany zrobione osobno mogą dać rozłączony łańcuch, który nigdzie nie zgłasza błędu.

## Znane ryzyko: szum w konwersjach z /contact

Konwersja "Marotino Lead - Contact Form" jest liczona **client-side** i dziedziczy znaną słabość.

**Mechanizm.** W `marotino_www_astro1`, `src/pages/contact.astro` (~linia 305), formularz odpala:

```js
const res = await fetch('/netlify-forms.html', { method: 'POST', ... });
if (res.ok) {
  window.dataLayer.push({ event: 'generate_lead', form_name: 'contact', ... });
}
```

To **dokładnie ten sam optymistyczny wzorzec**, który wyprodukował fantomowe konwersje przy Xenii (patrz sekcja z 05.09.2026). Komentarz w kodzie twierdzi, że fire następuje „only on a confirmed 2xx, never on optimistic submit" — ale **2xx z tego POST-a nie dowodzi, że Netlify zapisał zgłoszenie**. Netlify po cichu odrzuca spam i honeypot, a i tak zwraca 200. Ścieżka szumu: bot headless wypełnia prawdziwy formularz → Netlify odrzuca jako spam → zwraca 200 → JS liczy konwersję → Ads dostaje lead, którego nie ma w Netlify Forms.

**Dlaczego nie użyliśmy od razu ścieżki server-side.** Endpoint `src/pages/api/lead-conversion.ts` istnieje i jest odporny na ten problem (odpala się z webhooka Netlify Forms **dopiero po zapisaniu** zgłoszenia — to jedyne wywołanie, które dowodzi, że lead naprawdę był). Ale dziś:

- endpoint **jawnie ignoruje formularz `contact`**: `if (formName !== 'xenia-pilot') return new Response('OK (ignored form)')`,
- webhook w Netlify (Site configuration → Notifications → Form submission notifications) jest skonfigurowany **tylko dla formularza `xenia-pilot`**,
- formularz `/contact` **nie ma ukrytych pól `gclid` ani `ga_client_id`** (dodano je wyłącznie do `xenia-pilot` przy fixie z 05.09) — a bez `gclid` server-side `uploadClickConversions` nie ma czym przypiąć leada do kliknięcia, więc i tak by go pominął (`gclid ? upload : skip`),
- bramka kompletności w endpoincie wymaga niepustych `name` + `phone` + `email`, a formularz `/contact` **nie ma pola telefon** — więc w obecnej formie odrzuciłaby **każde** zgłoszenie z `contact`.

Czyli przepięcie na server-side to nie jest przełącznik, tylko cztery zmiany naraz. **Szum został świadomie zaakceptowany przez Cezarego na start (decyzja 14.09.2026)** — przy małym budżecie skala fantomów jest ograniczona, a alternatywą byłoby opóźnienie startu kampanii.

**Warunek tej zgody — cotygodniowa reconciliacja.** Raz w tygodniu porównać liczbę konwersji **"Marotino Lead - Contact Form"** w Google Ads z liczbą realnych zgłoszeń formularza **`contact`** w dashboardzie Netlify Forms. Rozjazd w górę po stronie Ads = fantomy. Jeśli rozjazd urośnie na tyle, że zaburza optymalizację, to sygnał, żeby przyspieszyć utwardzenie server-side zamiast dalej tolerować szum.

**Utwardzenie server-side — co konkretnie trzeba zrobić** (zadanie, nie zrobione):

1. Dodać do formularza `/contact` ukryte pola `gclid` i `ga_client_id`, wypełniane **tuż przed każdym submitem** (nie raz przy załadowaniu strony — `form.reset()` wyczyściłby je przy drugim wysłaniu w tej samej sesji; to pułapka złapana przy `xenia-pilot`).
2. Rozszerzyć `lead-conversion.ts` o obsługę `formName === 'contact'` — z **osobną bramką kompletności**, bo `contact` nie ma pola telefon (np. wymagać `email` + `name`, albo `email` + niepustego `message`).
3. Dodać osobną conversion action lub użyć istniejącej i przekazywać właściwe `GOOGLE_ADS_CONVERSION_ACTION_ID` per formularz (dziś jest jedno, zaszyte pod Xenię — `7742094210`).
4. Skonfigurować w Netlify **drugi outgoing webhook** dla formularza `contact`, wskazujący na `https://marotino.com/api/lead-conversion?key=<LEAD_WEBHOOK_SECRET>`.
5. Po przepięciu **usunąć client-side `dataLayer.push` z `contact.astro`**, żeby nie liczyć tej samej konwersji dwa razy — albo zostawić GTM-owy tor wyłącznie dla GA4, a Ads karmić tylko server-side.

## Co mamy na stronie pod reklamy (inwentarz z repo marotino_www_astro1)

Rekonesans zrobiony 14.09.2026 — spisany, żeby nie powtarzać przeglądania repo przy każdej zmianie kampanii.

**Strony docelowe najlepsze pod dolny lejek** (wysoka intencja zakupowa, konkretne liczby):

- **`/cost/custom-software-development`** — „Custom Software Development Cost USA". Transparentne widełki **$15K–$200K+** rozbite na typy projektów (prosty tool $15-40K, SaaS MVP $40-80K, enterprise $80-200K+), 5 pytań FAQ z realnymi odpowiedziami (co napędza koszt, fixed-price vs T&M, timeline'y), wzmianka o Miami w meta description. To najlepszy materiał reklamowy w całym repo.
- **`/cost/ai-development`** — „AI Development Cost USA 2026".
- **`/cost/saas-development`** — „SaaS Development Cost USA 2026".

**Mankament wszystkich trzech:** CTA to **link do `/contact`** („Start a Free Discovery Call"), czyli dodatkowe kliknięcie między reklamą a formularzem. Przy ruchu płatnym każdy dodatkowy krok kosztuje konwersje — warto zmierzyć, ile ruchu z `/cost/*` faktycznie dochodzi do `/contact`.

**Strony usługowe:** `/services/custom-software-development` ma hero z CTA **„Start Your Project" → `/contact`** plus drugie CTA na dole strony, oraz sekcję powiązanych case studies (Optienergia, Caz Investments, Asabik). Łącznie **14 stron `services/*`** i **6 stron `industries/*`** (fintech, e-commerce, medtech/healthtech, real estate/proptech, logistics, hospitality/food) — czyli materiał na dużo wąskich, dobrze dopasowanych grup reklam.

**Dowód lokalny pod Florydę** (to jest realna przewaga, nie marketingowa fikcja):

- **Marotino Inc, 66 West Flagler Street, Miami FL 33130** — realny adres w schema `Organization` na `/contact`.
- Profil **BBB Miami** (`bbb.org/us/fl/miami/profile/computer-software-developers/marotinoinc-...`) linkowany w `sameAs` na stronie głównej, obok Clutch i Trustpilot.
- Schema **`LocalBusiness`** / **`ProfessionalService`** z adresem FL na stronie głównej; strona `/about/locations` opisuje Marotino Inc jako podmiot amerykański obsługujący klientów z Ameryki Północnej i Południowej.

**Jedyne case study z Florydy — i bardzo dobre:**
**`/work/hlm-lakeland-hardwood-lumber-app`** — Hardwood Lumber and Millwork (**HLM Lakeland**), tartak z **Lakeland na Florydzie**, specjalizujący się w drewnie liściastym krajowym i egzotycznym. Aplikacja **Flutter** (iOS + Android), **39 gatunków drewna** zdigitalizowanych do interaktywnego katalogu z filtrowaniem, kalkulatorem tarcicy i trybem offline. Wyniki: **3000+ pobrań w pierwszym roku**, **-38% czasu odpowiedzi supportu**. Klient nie miał wcześniej żadnej infrastruktury cyfrowej.

To **najmocniejszy dowód społeczny do reklam na Florydę** — realna, lokalna firma produkcyjna, konkretne liczby, branża daleka od „startupowej", czyli wiarygodna dla lokalnego biznesu z FL.

**Luka do załatania:** **nie istnieje żadna strona z H1 pod lokalne frazy** w stylu „Software Development Company in Miami". Słowo „Miami" pojawia się w serwisie wyłącznie pobocznie — w schema, w adresie, w opisie lokalizacji — ale nigdy jako główny temat strony. To bezpośrednio obniża **Quality Score na frazach lokalnych**, czyli akurat na tych, które przy kampanii geo-targetowanej na Florydę są najtańsze i najlepiej konwertują (użytkownik szukający „software development company miami" ma wyższą intencję niż szukający „custom software development"). Google ocenia spójność query ↔ ad ↔ landing page; bez takiej strony reklama na frazę lokalną ląduje na stronie, która o lokalizacji nie mówi nic.

**Zadanie:** rozważyć zbudowanie `/miami-software-development` (albo `/locations/miami`) z H1 pod frazę lokalną, sekcją o Marotino Inc w Miami, case study HLM Lakeland jako dowodem i formularzem/CTA do `/contact`. To jest jedyna zmiana w serwisie, która realnie odblokowuje najtańszy segment ruchu — ale wymaga decyzji, bo powyżej zapadła decyzja „bez nowych landing page'ów".

---

## Research słów kluczowych (14.09.2026) — dlaczego Floryda nie wystarcza

Przed wydaniem pierwszego euro na kampanię 2 sprawdziliśmy realne wolumeny w Keyword Plannerze, zamiast zgadywać. Wynik **odwrócił wstępną decyzję o geo**.

**Kluczowe odkrycie: wąskim gardłem jest geo, nie budżet.** To nie jest niuans — to zmienia sens całej kampanii. Przy poprzednim założeniu (Floryda, wąskie frazy lokalne) żaden budżet nie dowiózłby leadów, bo nie ma czego kupić.

### Frazy lokalne — fundament pierwotnego planu, który się rozsypał

| fraza | Floryda | USA |
|---|---|---|
| software development company miami | **20** | 40 |
| custom software development miami | 10 | 10 |
| app development company miami | 110 | 170 |

Fraza, która miała być rdzeniem kampanii — `software development company miami` — ma **20 wyszukiwań miesięcznie na całej Florydzie**. Przy exact match i realnym udziale w wyświetleniach to kilka kliknięć **na miesiąc**.

### Integracje systemowe (USA, konkurencja Low)

| fraza | wolumen | stawka top-of-page (low–high) |
|---|---|---|
| salesforce integration services | 590 | €12.50–49.95 |
| system integration services | **390** | **€7.42**–33.60 |
| api integration services | 320 | €19.84–102.45 |
| data integration services | 320 | **€10.50**–30.33 |
| erp integration services | 110 | €14.60–37.89 |
| custom api development | 110 | €13.58–41.49 |
| shopify erp integration | 70 | €16.91–42.87 |
| third party api integration | 70 | — |
| netsuite integration partner | 50 | €18.99–96.01 |
| hubspot integration services | 40 | €8.92–36.49 |
| quickbooks integration services | 20 | — |

Najlepszy stosunek wolumenu do ceny wejścia: **`system integration services`** (390 wyszukiwań, wejście od €7.42) i **`data integration services`** (320, od €10.50). Zwróć uwagę na rozpiętość stawek — `api integration services` ma górną granicę **€102.45**, co jest bezpośrednim argumentem za twardym limitem max CPC (patrz ustawienia kampanii niżej).

### E-commerce (USA)

| fraza | wolumen | stawka top-of-page |
|---|---|---|
| hire shopify developer | 480 | €7.15–28.30 |
| shopify migration services | 320 | **€7.14**–19.92 |
| shopify developer near me | 210 | €7.98–40.63 |
| software maintenance services | 170 | €22.62–44.89 |
| shopify maintenance | 40 | — |
| woocommerce maintenance | 30 (Medium) | €22.43–46.86 |
| ecommerce maintenance services | 30 | — |

`shopify migration services` to **najtańsze wejście w całym researchu** (€7.14) przy przyzwoitym wolumenie 320 — i jednocześnie obszar z najmocniejszym portfolio (Batycki, Delta Marine, Ebikezilla, Loventi).

### MVP (USA) — wolumen jest, ale drogo

| fraza | wolumen | stawka top-of-page |
|---|---|---|
| mvp development services | 590 (Medium) | €21.14–86.25 |
| build an mvp | 480 | €5.46–42.40 |
| mvp development company | 480 | €25.84–83.99 |
| saas mvp development | 70 | €33.23–86.25 |

Odłożone na start. Wolumeny są dobre, ale wejście €21–33 przy budżecie €12/dzień oznacza mniej niż jedno kliknięcie dziennie. `build an mvp` jest tanie (€5.46), ale to w dużej mierze ruch **informacyjny** — ludzie czytają „jak zbudować MVP", nie szukają wykonawcy.

### Miami DMA — twarda odpowiedź na pytanie „czy da się zbierać leady tylko z okolic HQ"

Sprawdzone osobno dla **Miami-Ft. Lauderdale FL DMA** (zasięg 14,2 mln — obejmuje Dade + Broward + Palm Beach, czyli całą aglomerację Południowej Florydy):

Cały koszyk integracji to **~120 wyszukiwań miesięcznie** (`api integration services` 20, cała reszta po 10). To około **6% wolumenu USA**.

**Wniosek:** przy exact match daje to kilka–kilkanaście kliknięć miesięcznie. Kampania ograniczona do Miami **nie wydałaby nawet budżetu €12/dzień** — nie z powodu ustawień, tylko dlatego, że nie ma czego kupić. Popytu, którego nie ma, nie da się wyprodukować budżetem ani optymalizacją.

### Zero wolumenu — odrzucone mimo posiadanych case studies

`coupa punchout integration`, `punchout catalog provider`, `plaid integration developer` — **brak danych / zero wyszukiwań**.

To bolesne, bo Marotino ma pod te frazy realne, mocne case studies (Coupa PunchOut, CAZ Investments / Plaid). **Wniosek na przyszłość:** posiadanie świetnego dowodu w danej niszy nie znaczy, że ta nisza nadaje się na Google Ads. Nikt tego nie wyszukuje. To materiał do **outboundu** (targetowanie firm po technologii, LinkedIn, cold mail), nie do reklam w wyszukiwarce.

### Pułapka: Keyword Planner domyślnie pokazuje dane dla Cypru

Narzędzie ustawia lokalizację na **Cypr** — dziedziczy ją z **profilu płatności konta**, a nie z targetowania kampanii. Przy tym ustawieniu wszystkie frazy pokazują zera i myślniki, co wygląda jak „ta nisza nie istnieje".

**Zawsze sprawdzić i ręcznie przestawić lokalizację przed odczytaniem jakichkolwiek danych.** Pułapka jest podstępna, bo narzędzie nie ostrzega — po prostu zwraca zera. Co gorsza, **ustawienie lokalizacji potrafi wrócić do Cypru** przy tworzeniu nowego planu, więc weryfikować przy każdym planie osobno, nie raz na sesję.

### Decyzja geo (14.09.2026): całe USA + korekta stawek na Miami

Kampania celuje w **całe USA**, z **korektą stawek +50% na DMA Miami-Ft. Lauderdale**.

**Uzasadnienie:** Marotino Inc to podmiot amerykański obsługujący klientów zdalnie. „Miami-based" działa więc jako **wiarygodność w treści reklamy**, a nie jako ograniczenie zasięgu — nie ma powodu, żeby odcinać 94% rynku po to, by być lokalnym. Korekta +50% sprawia, że zapytanie z okolic HQ wygrywa aukcję częściej i wyżej, a reszta budżetu nie leży odłogiem. Raport geo w Google Ads pokaże, ile leadów faktycznie przyszło z Florydy — czyli decyzja jest mierzalna, nie jest wiarą.

### Realistyczna matematyka przy €12/dzień

Google w kreatorze kampanii wyestymował dla grupy integracyjnej **55 kliknięć/tydzień przy średnim CPC €4.42** — czyli **niżej, niż sugerowałyby stawki top-of-page** z tabel wyżej (te pokazują próg wejścia na górę strony, nie realną średnią).

Budżet €12/dzień = €360/mies. i tak ogranicza to do **~80 kliknięć miesięcznie**. Przy konwersji 3–5% daje to **1–2 leady miesięcznie**.

**Zapisane wprost, żeby nie było nieporozumień przy ocenie wyników:** Google Ads przy tym budżecie jest **dodatkiem do lejka, a nie „pewnym źródłem sprzedaży"**. Realnym źródłem leadów pozostaje **outbound** (Twenty CRM, Apollo). Jeśli za miesiąc kampania dowiezie 1–2 leady, to jest **wynik zgodny z prognozą**, a nie porażka — i odwrotnie, oczekiwanie stałego strumienia zapytań przy €12/dzień byłoby oczekiwaniem wbrew arytmetyce.

---

## Kampania 2 — stan budowy (14.09.2026, NIEDOKOŃCZONA)

Utworzony **draft** kampanii **"Marotino - Integrations & Ecommerce - US Search"** (`campaignId 281499226399188`, `draftId 10213925383`).

### Ustawione i zapisane

- **Typ Search**, utworzona przez **"Create a campaign without guidance"** — świadomie, żeby kreator z celem „Leads" nie stworzył znowu widmowej akcji **"Lead form - Submit"**, jak przy Xenii (patrz incydent 31.08.2026).
- **Odznaczone Google Display Network oraz Google Search Partners.** Display — bo nie ma assetów graficznych. Search Partners — bo przy €12/dzień to słabszy jakościowo ruch, a każde kliknięcie jest policzalne. (Przy Xenii odznaczony był tylko Display.)
- **Odznaczone „Website visits" i „Phone calls"** w kroku wyboru rezultatów — żeby kreator nie wymusił assetów telefonicznych ani lead form.
- **Licytacja: „Clicks" z limitem max CPC €8** zamiast domyślnego **"Maximize conversions"**. To jest bezpośrednie zastosowanie lekcji z 31.08.2026 — Maximize conversions bez danych konwersji zdusiło Xenię do zera impresji. Limit €8 chroni dodatkowo przed górnymi stawkami rzędu €100 (patrz `api integration services` wyżej).
- **Geo:** United States **+ Miami-Ft. Lauderdale FL DMA**. Druga pozycja jest dodana **wyłącznie po to, by móc nałożyć na nią korektę stawek +50%** — bez wpisania jej jako osobnej lokalizacji nie ma do czego przypiąć korekty.
- **AI Max: wyłączony.** Chcemy kontroli exact match, a nie rozszerzania zapytań przez AI — przy tym budżecie każde niedopasowane kliknięcie to znaczący procent dziennego wydatku.
- **EU political ads:** odpowiedziane „No" na poziomie kampanii. (Uwaga: to **nie zastępuje** deklaracji na poziomie konta — ta była ukrytym blockerem Xenii, patrz sekcja z 01.09.2026, i jest już odpowiedziana.)
- **Pominięte AI-generowanie słów kluczowych i assetów** — przy Xenii wyprodukowało bezsensowne nagłówki („Without Your Own Channel", „You Have Twenty Minutes") z oceną Ad strength „Poor".

### Grupa reklam 1 — „integracje"

- **Frazy:** `[system integration services]`, `[data integration services]`, `[api integration services]`, `[erp integration services]`, `[custom api development]`, `"system integration services"`, `"data integration services"` — wyłącznie exact i phrase, zero broad.
- **Landing:** `/services/custom-software-development`.
- **Reklama RSA — 7 nagłówków:** System Integration Experts / Custom API & Data Integration / ERP, CRM & Payment Systems / Miami-Based Engineering Team / Free 30-Min Discovery Call / Fixed-Price Scope & Quote / Full Source Code Ownership. Plus 2 opisy i business name „Marotino".
- **Ad strength: „Poor"** — do poprawy. Brakuje nagłówków zawierających **dosłowne frazy kluczowe** (Google wprost podpowiada „Try including more keywords in your headlines"). Przy Xenii ten sam problem rozwiązało wstawienie treści wyciągniętych z realnej strony produktowej — ocena podskoczyła z „Poor" na „Average".

### Zablokowane na kroku budżetu: "Confirm it's you"

Google wyświetlił okno **„Confirm it's you"** (ponowna weryfikacja tożsamości / 2FA) **w środku kreatora kampanii**, na kroku budżetu. Jednocześnie w lewym panelu pojawiło się **„Changes failed to save"** oraz ostrzeżenie przy kroku Bidding.

Tego okna **nie da się przejść automatycznie** — wymaga człowieka, dokładnie tak jak przy zakładaniu konta (patrz pułapka nr 5 w sekcji „Pułapki podczas zakładania konta").

**Wniosek na przyszłość — dwa osobne:**

1. Google potrafi wyrzucić re-auth **w środku kreatora kampanii**, nie tylko przy operacjach płatniczych. Planując automatyzację budowy kampanii, założyć, że sesja może zostać przerwana w dowolnym kroku.
2. Po takiej blokadzie **zweryfikować, czy wcześniejsze kroki faktycznie się zapisały** — komunikat „Changes failed to save" sugeruje, że część zmian przepadła. W tym przypadku krytyczny do sprawdzenia jest **limit max CPC €8**: jeśli nie zapisał się, kampania po uruchomieniu licytowałaby bez sufitu, przy górnych stawkach sięgających €100 za kliknięcie.

### Do dokończenia po weryfikacji tożsamości

1. **Budżet €12/dzień sztywno** — przez **„Set custom budget"**. Google podpowiada €27.81 / €34.76 / €41.71 — **zignorować wszystkie trzy**.
2. **Korekta stawek +50%** na DMA Miami-Ft. Lauderdale.
3. **Druga grupa reklam — e-commerce/Shopify:** `[shopify migration services]`, `[hire shopify developer]`, `[shopify developer near me]`, landing `/services/e-commerce-solutions`.
4. **Negatywne słowa kluczowe** (od pierwszego dnia, czego zabrakło przy starcie Xenii): jobs, salary, hiring, careers, intern, resume, course, tutorial, learn, bootcamp, free, cheap, github, open source, udemy, coursera, freelance, upwork, fiverr, student, degree, certification, entry level, remote jobs.
5. **Sitelinki** (pamiętać o unikalnych final URL per sitelink — Google odrzuca duplikaty).
6. **Poprawa Ad strength** z „Poor" — dodać nagłówki z dosłownymi frazami kluczowymi.
7. **Zweryfikować, czy limit max CPC €8 przetrwał** blokadę (patrz wyżej).
8. **Zostawić kampanię zapauzowaną** do akceptacji Cezarego przed startem.

---

## Kampania 2 — URUCHOMIONA (14.09.2026)

Wszystkie punkty z listy powyżej zostały wykonane tego samego dnia, po przejściu weryfikacji tożsamości przez Cezarego. Kampania jest **LIVE** (Enabled), ID `24249790952`, start 14.09.2026.

### Stan końcowy

| ustawienie | wartość |
|---|---|
| Budżet | **€12.00/dzień** (sztywno, przez „Set custom budget") |
| Licytacja | Maximize clicks z **limitem max CPC €8** — zweryfikowane, że przetrwał blokadę |
| Sieci | tylko Google Search (bez Display, bez Search Partners) |
| Geo | United States + DMA Miami-Ft. Lauderdale FL |
| **Location options** | **„Presence"** — patrz niżej, kluczowa zmiana |
| Korekta stawek | **+50% na DMA Miami-Ft. Lauderdale** |
| Grupy reklam | 2 (integracje + e-commerce), łącznie 14 fraz exact/phrase |
| Negatywy | **+44 frazy** na poziomie kampanii (konto ma teraz 116) |
| Optimization score | 89.8% |

### Grupa reklam 2 — „Ecommerce - Shopify"

- **Frazy:** `[shopify migration services]`, `[hire shopify developer]`, `[shopify developer near me]`, `[shopify erp integration]`, `[shopify maintenance]`, `"shopify migration services"`, `"hire shopify developer"`.
- **Landing:** `/services/e-commerce-solutions`.
- **12 nagłówków**, m.in. Shopify Migration Services / Hire Shopify Developers / Shopify ERP Integration / Shopify Maintenance & Support / Replatform Without Downtime / 4 Live Ecommerce Case Studies. Dwa opisy własne, business name „Marotino".
- Google potwierdził przy zapisie: „reviewed the ad that you just saved and found no policy issues".
- **Ad strength: „Poor"** — nadal do poprawy (grupa 1 udało się podnieść do „Average" po dodaniu trzech nagłówków z dosłownymi frazami: System Integration Services / Data Integration Services / API Integration Services).

### Location options: „Presence" zamiast „Presence or interest" — WAŻNE

Google domyślnie ustawia **„Presence or interest: People in, regularly in, or who've shown interest in your included locations (recommended)"**. Oznacza to, że reklamy widzą **także osoby spoza USA**, które jedynie wykazały „zainteresowanie" USA — w praktyce furtka dla klików z Indii, Nigerii czy Pakistanu przy kampanii celowanej w USA.

Przestawione na **„Presence: People in or regularly in your included locations"**.

**Wniosek na przyszłość:** negatywne słowa kluczowe **nie rozwiązują** tego problemu — negatyw filtruje treść zapytania, a nie kraj użytkownika. Jedyny właściwy fix to ustawienie Location options na „Presence". Robić to przy starcie **każdej** kampanii celowanej geograficznie.

### Śledzenie konwersji — zweryfikowane end-to-end na żywo

Test wykonany 14.09.2026 na `marotino.com/contact`: wstrzyknięcie `dataLayer.push({event:'generate_lead', form_name:'contact'})` i podsłuch ruchu sieciowego. Potwierdzony pełny łańcuch:

1. GTM ładuje trzy kontenery: `GTM-5JRBQF9N`, `G-JT28P4C0LD` (GA4), `AW-18418762437` (Ads).
2. GA4 dostaje hit `g/collect` z `en=generate_lead` i `ep.form_name=contact`.
3. Google Ads dostaje hit `googleadservices.com/pagead/conversion/18418762437/?...&label=xLxkCPaHqPccEMWF4M5E` — dokładnie ID i label akcji „Marotino Lead - Contact Form". Plus `ccm/conversion` i `viewthroughconversion` na tym samym ID.
4. Zgoda na cookies: `gcs=G111` (udzielona).

**Uwaga:** ten test wysłał **realny sygnał konwersji** do konta. W raportach pojawi się 1 konwersja bez przypisanej kampanii (brak `gclid`, bo nie było kliknięcia reklamy). To artefakt testu, nie lead.

### Diagnostyka po starcie

Campaign diagnostics pokazuje: Campaign published ✓, **Account ✓, Ads ✓, Goals ✓**, Impressions ⚠️ z jednym ostrzeżeniem **„Missing enough relevant keywords"**, Clicks: Upcoming.

Warto to zestawić ze startem Xenii, gdzie równolegle występowały cztery blokery: „missing Google tag", „Eligible (Misconfigured)", błąd płatności i niedziałający conversion tracking. Tutaj **żaden z nich nie wystąpił** — jedyne ostrzeżenie to świadomy kompromis: wąska lista exact/phrase przy budżecie €12/dzień. Wolimy mało trafnych kliknięć niż dużo przypadkowych.

### Pułapki UI napotkane przy budowie (żeby nie tracić na nie czasu ponownie)

1. **Ikona edycji korekty stawek w tabeli Locations wymaga prawdziwego najechania myszą** — syntetyczne zdarzenia `mouseover`/`mousemove` jej nie pokazują. Obejście: zaznaczyć checkbox wiersza i użyć paska akcji zbiorczych **Edit → Change bid adjustments**, który ma normalny, zawsze widoczny przycisk Apply.
2. **Keyword Planner domyślnie ustawia lokalizację na kraj profilu płatności (Cypr), nie na geo kampanii** — bez ręcznej zmiany pokazuje same zera i wnioski są bezwartościowe.
3. **Komunikat „Turn off ad blockers" jest stale obecny w DOM strony Google Ads**, niezależnie od tego, czy blocker istnieje. Nie diagnozować po nim problemów z zapisem — sprawdzać realny stan pól po odświeżeniu.
4. **Ekran „Review" w kreatorze potrafi pokazywać nieaktualne dane** (np. „Ads: None" mimo poprawnie zapisanej reklamy, „Text customization turned on" mimo odznaczonych checkboxów). Weryfikować przez wejście w konkretny krok, nie przez podsumowanie.

### Zostało do zrobienia

- [ ] Podnieść **Ad strength grupy 2** z „Poor" (dodać nagłówki z dosłownymi frazami, jak zadziałało w grupie 1).
- [ ] **Sitelinki** — brak w obu grupach (pamiętać o unikalnych final URL per sitelink).
- [ ] **Utwardzenie server-side trackingu dla `contact`** — obecnie konwersje idą wyłącznie client-side i dziedziczą ryzyko fantomów opisane w sekcji „Znane ryzyko: szum". Cotygodniowo porównywać liczbę konwersji „Marotino Lead - Contact Form" w Ads z realnymi zgłoszeniami formularza `contact` w Netlify Forms.
- [ ] Po tygodniu: **przegląd search terms** i dosypanie negatywów (przy Xenii tydzień 1 pokazał ~€21 spalone na frazach „ai for business" bez kontekstu).
- [ ] **Advertiser verification** przed **2026-10-07** — dotyczy całego konta, więc blokuje też tę kampanię.

---

## Kampania 2 — pierwszy check-in po 1 dniu (15.09.2026)

Dane odczytane z UI (brak API), zakres **All time** dla kampanii `24249790952`, czyli faktycznie 14–15.09.

### Liczby

| metryka | wartość |
|---|---|
| Impresje | **33** |
| Kliknięcia | **2** |
| CTR | 6.06% |
| Śr. CPC | **€3.44** |
| Koszt | **€6.89** |
| Konwersje | **0** |
| Status | **Eligible (Limited)** — „Missing enough relevant keywords" |

Dla porównania, Xenia w swoim ostatnim tygodniu (8–14.09, ostatni dzień serwowania to 14.09: 116 impr., 29 klików, €20.93) miała śr. CPC **€0.87**. Nowa kampania jest więc **~4× droższa za kliknięcie** — co jest spodziewane (B2B usługi vs nisza hotelowa), ale trzeba to czytać razem z prognozą z sekcji „Realistyczna matematyka przy €12/dzień": tam Google estymował €4.42, a €3.44 mieści się poniżej tej estymaty. **Limit max CPC €8 nie jest obecnie blokerem** — realna stawka jest od niego mocno niższa.

### Najważniejsze odkrycie: cały ruch idzie z JEDNEGO słowa, i to nie z tego, o które nam chodziło

Rozbicie po słowach kluczowych (All time):

| słowo | impr. | kliki | koszt |
|---|---|---|---|
| `"system integration services"` (phrase) | **27** | **2** | **€6.89** |
| `"data integration services"` (phrase) | 5 | 0 | €0.00 |
| **wszystkie 7 fraz exact w grupie 1** | **0** | 0 | €0.00 |
| **cała grupa „Ecommerce - Shopify" (7 fraz)** | **0** | 0 | €0.00 |

Czyli **100% wydatku pochodzi z dwóch fraz phrase match, a exact match nie zebrał ani jednej impresji**. Grupa e-commerce — ta, pod którą mamy najmocniejsze portfolio (Batycki, Delta Marine, Ebikezilla, Loventi) i najtańsze wejście w całym researchu (€7.14 na `shopify migration services`) — **nie wystartowała wcale**.

### I odkrycie gorsze: phrase match łapie przemysłową automatykę, nie integracje software'owe

Search terms report (te, które Google w ogóle pokazuje — 6 z 32 impresji, reszta ukryta jako „Other search terms"):

| zapytanie | impr. |
|---|---|
| `fanuc system integrator` | 3 |
| `am system integrations` | 1 |
| `automation systems integrator` | 1 |
| `integration in salesforce` | 1 |

**FANUC to producent robotów przemysłowych.** „Systems integrator" w USA to utrwalony termin z branży **automatyki fabrycznej / robotyki** — firmy, które instalują linie produkcyjne, a nie łączą API. Fraza `"system integration services"`, wybrana w researchu jako najlepszy stosunek wolumenu do ceny (390 wyszukiwań, wejście €7.42), sprowadza więc w dużej mierze **ruch z zupełnie innej branży**. Te 390 wyszukiwań/mies. to nie jest 390 zapytań o software house.

To bezpośrednio wyjaśnia, dlaczego exact match ma zero: `[system integration services]` w dokładnym brzmieniu prawie nikt nie wpisuje — wolumen z Keyword Plannera rozkłada się na warianty przemysłowe, które łapie tylko phrase.

**Wniosek na przyszłość (nowy, nie było go w researchu):** wolumen z Keyword Plannera nie mówi nic o **intencji branżowej** za frazą. Przy frazach, które mogą mieć homonim w innej branży (integration, automation, systems, solutions), przed uruchomieniem sprawdzić realne SERP-y albo od razu przygotować negatywy branżowe. Research z 14.09 sprawdził wolumen, cenę i konkurencję — ale nie sprawdził, **kto** to wyszukuje.

### Co z tego wynika operacyjnie

1. **Dosypać negatywy przemysłowe** — `fanuc`, `robot`, `robotics`, `plc`, `scada`, `industrial`, `automation integrator`, `systems integrator` (jako negatyw phrase), `manufacturing`. To jest pilniejsze niż standardowy przegląd search terms po tygodniu, bo to nie szum na marginesie, a **główne źródło ruchu**.
2. **Nie panikować przy „Eligible (Limited) / Missing enough relevant keywords"** — to ten sam świadomy kompromis, który zapisaliśmy przy starcie (wąska lista exact/phrase przy €12/dzień). Ale w połączeniu z faktem, że exact ma 0 impresji, komunikat przestaje być tylko kosmetyczny: przy tak wąskiej liście realny ruch kupujemy **wyłącznie** przez 2 frazy phrase, i to te niedopasowane.
3. **Zbadać, dlaczego grupa Shopify ma 0 impresji.** Hipoteza: limit max CPC €8 jest ustawiony dokładnie na progu wejścia dla tych fraz (`hire shopify developer` €7.15–28.30, `shopify developer near me` €7.98–40.63) — czyli formalnie mieścimy się w dolnej granicy, ale przegrywamy każdą aukcję. Do sprawdzenia w kolumnach **Search impr. share (lost to rank)** albo przez symulator stawek, gdy uzbiera się dane. Jeśli hipoteza się potwierdzi, to jest realne napięcie między „twardy limit €8 chroni przed stawkami €100" a „przy €8 nie kupimy najlepszej grupy słów".
4. **0 konwersji po 2 kliknięciach nie znaczy nic** — przy prognozie 1–2 leady/mies. brak konwersji przez pierwsze dni jest w pełni zgodny z arytmetyką z sekcji wyżej. Nie wyciągać wniosków o skuteczności przed ~50 kliknięciami.

### Poprawka do dokumentacji

Grupa reklam 1 nazywa się w koncie faktycznie **„Ad group 1"** (nazwa domyślna z kreatora), a nie „integracje" — w README figurowała pod nazwą opisową. Warto ją przemianować, żeby raporty były czytelne, skoro grup jest już dwie.

---

## Audyt + interwencja nocna (15.09.2026)

Pełny audyt konfiguracji kampanii 2 po pierwszym dniu, z natychmiastowym wdrożeniem poprawek. Kontekst: w USA była noc, celem było złapanie sensownego pace'u na amerykański poranek.

### Co audyt potwierdził jako poprawne (nie ruszane)

| element | stan | wniosek |
|---|---|---|
| Limit max CPC | **€8.00, faktycznie zapisany** | Punkt 7 z listy „do dokończenia" z 14.09 zamknięty — blokada „Confirm it's you" nie zjadła tego ustawienia. Realna śr. CPC €3.44 jest mocno pod limitem, **więc limit nie jest obecnie blokerem** i świadomie go NIE podnosimy: przy €12/dzień klik za €8 to 1.5 klika dziennie, podnoszenie sufitu byłoby walką z arytmetyką. |
| Sieci | tylko Google Search | bez Display, bez Search Partners — zgodnie z planem |
| AI Max | wyłączony | zgodnie z decyzją z 14.09 |
| Location options | Presence | zgodnie z poprawką z 14.09 |
| Harmonogram reklam | całodobowy, bez korekt | patrz uwaga o strefie czasowej niżej |
| Conversion goals | Account-default: Submit lead forms | OK; przy Maximize clicks nie ma to jeszcze wpływu na licytację |

### Nowa pułapka strukturalna: Maximize clicks + fraza z homonimem = systematyczny zakup złego ruchu

To jest właściwa diagnoza tego, co widzieliśmy w check-inie z rana. Nie chodziło tylko o to, że `"system integration services"` łapie automatykę przemysłową. Chodzi o **interakcję dwóch decyzji, z których każda osobno była słuszna**:

1. **Maximize clicks** wybrano świadomie (lekcja z 31.08: Maximize conversions bez danych konwersji zdusiło Xenię do zera). Dobra decyzja.
2. Fraza phrase `"system integration services"` weszła na podstawie researchu wolumenu. Też wygląda na dobrą decyzję.

Ale Maximize clicks optymalizuje **liczbę kliknięć, nie ich trafność**. Znalazł najtańszą kieszeń ruchu w kampanii — zapytania o integratorów automatyki fabrycznej, gdzie software house nie konkuruje z nikim i klik jest tani — i wrzucił tam **cały** budżet. Dlatego grupa Shopify miała 0 impresji: nie dlatego, że przegrywała aukcje, ale dlatego, że **algorytm nigdy do niej nie doszedł**, mając tańsze kliki obok.

**Wniosek na przyszłość:** przy Maximize clicks każda fraza, która może łapać inną branżę, jest nie tylko źródłem szumu — jest **magnesem na cały budżet**. Negatywy branżowe przy tej strategii licytacji nie są kosmetyką po tygodniu, tylko warunkiem, żeby kampania w ogóle testowała to, co chcieliśmy testować.

### Wykonane zmiany

**1. Negatywy branżowe — 34 frazy, poziom kampanii (44 → 78)**

`fanuc`, `kuka`, `yaskawa`, `motoman`, `abb robot`, `rockwell`, `allen bradley`, `robot`, `robots`, `robotic`, `robotics`, `plc`, `plcs`, `scada`, `hmi`, `servo`, `cnc`, `conveyor`, `pneumatic`, `welding`, `machine vision`, `industrial automation`, `factory automation`, `automation integrator`, `automation integrators`, `systems integrator`, `systems integrators`, `system integrator`, `system integrators`, `controls integrator`, `control system integrator`, `panel builder`, `assembly line`, `manufacturing plant`.

Dwie uwagi projektowe:

- **`system integrator` / `systems integrator` jako negatyw NIE blokuje naszego `"system integration services"`** — negatyw broad wymaga obecności wszystkich swoich słów w zapytaniu, a „integrator" to inne słowo niż „integration". To jest najważniejszy negatyw z całej listy, bo trafia w rdzeń problemu.
- **Negatywy nie łapią bliskich wariantów** (inaczej niż słowa kluczowe pozytywne) — dlatego na liście są osobno `robot`/`robots`/`robotic`/`robotics` i `plc`/`plcs`. Wpisanie samego `robot` zostawiłoby „robotics" przepuszczone.
- Świadomie **pominięto** `siemens` i `abb` jako samodzielne negatywy — te firmy robią też software/PLM, a zapytanie o integrację Siemensa może być realnym leadem. Zablokowany jest tylko `abb robot`.

**2. Rozbudowa słów kluczowych — 26 nowych (14 → 40)**

Grupa `Integrations - API & ERP` (+14): frazy phrase dla tych, które miały tylko exact (`"api integration services"`, `"erp integration services"`, `"custom api development"`), plus zwalidowane w researchu z 14.09 a nieużyte: `salesforce integration services` (590 wyszukiwań!), `third party api integration`, `hubspot integration services`, `quickbooks integration services` — każde exact + phrase gdzie ma to sens. Dodatkowo `[software integration services]` / `"software integration services"`, `[erp integration company]`, `[api development company]`.

Grupa `Ecommerce - Shopify` (+12): `"shopify developer near me"`, `"shopify maintenance"`, `"shopify erp integration"` (grupa miała 5 z 7 fraz tylko w exact — to współprzyczyna zera impresji), plus `shopify api integration` (exact+phrase), `[shopify app development]`, `[custom shopify development]`, `shopify integration services` (exact+phrase), `[shopify development company]`, `[shopify plus agency]`, `[ecommerce maintenance services]`.

**Uczciwe oznaczenie:** `salesforce integration services` ma próg wejścia €12.50, powyżej naszego limitu €8 — dodane świadomie, bo limit działa jako twardy sufit (najwyżej nie wygramy aukcji, nie przepłacimy), a 590 wyszukiwań to największa pojedyncza pula w całym researchu. `[erp integration company]`, `[api development company]`, `[shopify development company]`, `[shopify plus agency]`, `[custom shopify development]`, `[shopify app development]` **nie były w researchu z 14.09** — to dodatki na intencji, do weryfikacji po pierwszych danych.

Wszystkie 26 weszły w status **Pending / under review** — to normalna weryfikacja nowych słów, przechodzi zwykle w kilka godzin, czyli dokładnie w nocnym okienku. Żadne nie zostało odrzucone ani oflagowane jako „Low search volume".

**3. Assety reklamowe — z 1 do 14 (kampania miała tylko business name!)**

Audyt pokazał, że kampania od startu miała **dokładnie jeden asset**: business name „Marotino" (nadal `Pending` od 14.09, 1:15 PM). Zero sitelinków, zero callotów, zero structured snippets. Google flagował to dwoma osobnymi rekomendacjami.

- **6 sitelinków** (każdy z 2 liniami opisu, wszystkie final URL-e sprawdzone `curl`em na 200, wszystkie unikalne): Free Discovery Call → `/contact`, Maintenance & Support → `/services/maintenance-support`, Legacy Modernization → `/services/legacy-modernization`, How We Work → `/process`, AI & Data Engineering → `/services/ai-data-engineering`, Fintech & Payments → `/services/fintech-solutions`. Google w formularzu wprost mówi: 2 wystarczą do wyświetlania, 6+ maksymalizuje performance — dlatego od razu 6.
- **6 callotów**: Miami-Based Team, Fixed-Price Scope, Senior US Engineers, Free 30-Min Discovery, Full Code Ownership, Documented APIs.
- **1 structured snippet**, header `Service catalog`, 6 wartości: System Integration, API Development, ERP & CRM Integration, Shopify Migration, Ecommerce Development, Maintenance & Support.

To nie jest kosmetyka — rozszerzenia powiększają powierzchnię reklamy w SERP-ie, podnoszą CTR, a CTR wchodzi do Ad Rank. Przy tym budżecie tańszy klik z lepszego Quality Score jest realnym mechanizmem na więcej ruchu za te same €12.

**4. Ad strength obu reklam**

Tu audyt **odwrócił to, co było w README**: to grupa 1 miała „Poor", a grupa 2 „Average" — nie na odwrót. Poprawione wyżej w tym dokumencie.

- Grupa 1: dodane 3 nagłówki z dosłownymi nowymi frazami (Salesforce Integration, Third Party API Integration, HubSpot & QuickBooks APIs), 3. opis, oraz **wypełnione puste Path 1/Path 2** → display URL to teraz `marotino.com/integrations/api-erp`. Wynik: **Poor → Average**. (W edytorze wskaźnik pokazywał „Good", po zapisie Google przeliczył na „Average" — wiążąca jest wartość po zapisie.)
- Grupa 2: dodane 3 nagłówki (Shopify Developer Near Me, Shopify API Integration, Custom Shopify Development), 3. opis, ścieżka `marotino.com/shopify/development`. W edytorze **„Excellent"**, po zapisie status przeliczania. Google przy obu zapisach potwierdził brak zastrzeżeń policy.

**Pułapka UI (nowa):** przycisk zapisu edytora reklamy to **„Save ad"**, nie „Save" — w DOM-ie jest jednocześnie kilkanaście nieaktywnych, ukrytych przycisków „Save" należących do innych paneli. Skrypt szukający pierwszego `Save` trafia w wyłączony i zapis cicho nie następuje. Druga pułapka: **ikona edycji reklamy pojawia się tylko po prawdziwym najechaniu myszą** (to samo co przy korektach stawek w Locations, patrz 14.09) — syntetyczne `mouseover` nie działa, trzeba realnego zdarzenia CDP. ID-ki reklam, gdy hover nie chce współpracować: grupa 1 `adId 824543077086` / `adGroupId 199848442949`, grupa 2 `adId 824544321969` / `adGroupId 200223584557`; edytor otwiera się bezpośrednio przez `/aw/ads/edit/search?...&adId=...&adGroupIdForAd=...`.

**5. Nazwa grupy reklam**

`Ad group 1` → **`Integrations - API & ERP`**. Pułapka: inline edytor nazwy ma własny przycisk Save w popupie; globalne szukanie przycisku „Save" na stronie klika coś innego i nazwa cicho się nie zapisuje (zdarzyło się przy pierwszej próbie). Trzeba scope'ować szukanie do kontenera inputa.

### Znalezione, świadomie NIE zmienione

- **Budżet został na €12/dzień.** To decyzja pieniężna Cezarego, nie techniczna. Przy śr. CPC €3.44 to ~3.5 klika dziennie — i to jest realny sufit pace'u, niezależnie od tego, jak dobrze ustawiona jest kampania. Jeśli oczekiwanie to „dobry pace" w sensie wolumenu, a nie trafności, to jedyna dźwignia to budżet.
- **Bid strategy została na Maximize clicks** — zgodnie z lekcją z 31.08 nie wracamy na Maximize conversions dopóki nie ma realnych danych konwersji.
- **Nie dodano harmonogramu reklam**, mimo że dla B2B kuszące jest ograniczenie do godzin pracy w USA. Powód: kampania jest **głodna impresji, nie przepalająca budżet na złe godziny** — zawężanie teraz dokręcałoby ograniczenie, które nie jest wąskim gardłem. Do rozważenia, gdy będą dane per godzina.
- **Grupy nie zostały rozdzielone na osobne kampanie.** Rozdzielenie dałoby grupie Shopify gwarantowany budżet, ale €6+€6 zagłodziłoby obie. Najpierw negatywy — jeśli po nich Shopify nadal ma 0 impresji, wtedy rozdzielenie staje się uzasadnione.

### Nowe ustalenie o koncie: strefa czasowa vs rynek — i tego nie da się naprawić

Konto ma strefę **(GMT+03:00) Eastern European Time**, bo tak zostało założone (profil płatności Marotino CY LTD). Kampania celuje w USA. Oznacza to, że **dzień budżetowy zaczyna się i kończy o 17:00 czasu wschodniego USA**, czyli **przecina amerykański dzień pracy na pół**: poniedziałek 9:00–17:00 ET leży w dwóch różnych dniach budżetowych.

**Strefy czasowej konta Google Ads nie można zmienić po założeniu.** To trwała konsekwencja setupu cypryjskiego — ta sama decyzja, która wcześniej wygenerowała ukryty bloker z formularzem EU political ads (01.09) i zera w Keyword Plannerze (14.09). Nie jest to katastrofa (Google rozkłada budżet według przewidywanego ruchu w dobie, nie wypala go na starcie), ale przy ocenie dziennych wyników trzeba pamiętać, że „dzień" w raportach to nie amerykański dzień pracy.

### Czego się spodziewać na rano — i jak to czytać

Realistycznie: nowe słowa wychodzą z review w kilka godzin, negatywy działają natychmiast, assety po zatwierdzeniu. Więc na amerykański poranek kampania powinna mieć **istotnie szerszą pulę aukcji i odcięty ruch przemysłowy**.

**Czego NIE traktować jako sukcesu ani porażki:**

- Wzrost impresji sam w sobie nic nie znaczy — patrzeć na **search terms**, czy zapytania wyglądają jak software, nie jak fabryka.
- Możliwy **wzrost śr. CPC powyżej €3.44** i to będzie **dobry znak**: znaczy, że kupujemy trafniejszy, droższy ruch zamiast taniego przemysłowego. Prognoza Google dla tej kampanii to €4.42, więc ruch w tę stronę jest zgodny z planem.
- Spadek liczby kliknięć przy tym samym budżecie jest **oczekiwany** i nie jest regresem.
- Status `Eligible (Limited) / Missing enough relevant keywords` może utrzymać się jeszcze kilka godzin — przeliczy się po wyjściu 26 nowych słów z review.

### Zostało do zrobienia (aktualizacja)

- [ ] **Rano: przegląd search terms** — czy negatywy odcięły FANUC i spółkę, i czy pojawiły się realne zapytania software'owe. To jedyny miarodajny test całej tej interwencji.
- [ ] **Sprawdzić, czy grupa Shopify wreszcie ma impresje.** Jeśli nadal 0 po dniu z negatywami i phrase matchem — dopiero wtedy hipoteza o limicie €8 / rozdzieleniu kampanii, i sprawdzić **Search impr. share (lost to rank)** w kolumnach.
- [ ] **Zweryfikować status 26 nowych słów** (Pending → Eligible) i 13 nowych assetów (Pending → Approved).
- [ ] Podnieść Ad strength grupy 1 z „Average" do „Good" — zostały wolne slaty nagłówków (13 z 15).
- [ ] Business name „Marotino" wisi w `Pending` od 14.09 — jeśli za kilka dni nadal nie będzie zatwierdzony, zgłosić.
- [ ] Rozważyć harmonogram reklam pod godziny pracy w USA — **dopiero gdy będą dane per godzina**.
- [ ] **Advertiser verification przed 2026-10-07** — nadal otwarte, dotyczy całego konta.

---

## Research geo — wybór rynku na kolejną kampanię (15.09.2026)

Dane z Keyword Plannera, drafty: `planId 1438165357` (15 fraz EN × 14 lokalizacji), `1438136905` i `1437986684` (36 fraz PL dla Warszawy). Drafty są nieczytelne po kilku dniach — ta sekcja jest źródłem prawdy.

**Metoda:** jeden stały koszyk 15 angielskich fraz (`api/data/erp/system integration services`, `custom api development`, `salesforce integration services`, `shopify migration services`, `hire shopify developer`, `shopify developer`, `software maintenance services`, `mvp development services`, `custom software development`, `software house`, `it outsourcing`, `mobile app development company`) puszczony kolejno na każdą lokalizację. Angielski świadomie — Marotino ma gotowe angielskie reklamy i landingi, więc pytanie brzmi „gdzie nasz istniejący creative zadziała", nie „gdzie jest popyt w dowolnym języku".

### Ranking

| # | rynek | wolumen/mies. | najtańsze wejście | czym atakować |
|---|---|---|---|---|
| 1 | **USA** (działa) | **17 500** | €4.46 | wszystko; jedyny rynek z realnym klastrem integracyjnym (1 840) |
| 2 | Wielka Brytania | 3 140 | €2.91 | Shopify (880) |
| 3 | Kanada | 1 620 | €2.54 | Shopify (480) + mobile (260), wszystko Low competition |
| 4 | Australia | 1 480 | €3.44 | Shopify (480), +23% kwartalnie |
| 5 | **ZEA** | 1 420 | **€1.10** | mobile apps (**1 000**, Low, **+22% r/r**) |
| 6 | Holandia | 610 | €2.96 | tylko Shopify (320) |
| 7 | Arabia Saudyjska | 550 | **€0.97** | mobile (260, drogo €17.42), +24% r/r |
| 8 | Singapur | 460 | €3.06 | custom software dev (90, +150% r/r) |
| 9 | Irlandia | 220 | €4.73 | tylko jako dodatek do UK |
| 10 | Polska / Warszawa | 880 (PL) | €1.22 | `outsourcing it` (210) |

**Odrzucone jako martwe po angielsku:** Wiedeń ~120, Tel Aviv ~90, Nikozja ~60. Google nie podał dla Wiednia i Nikozji **ani jednej stawki** — za mało danych, żeby cokolwiek szacować. Nikozja ma 645 tys. zasięgu, więc to było przewidywalne; Wiedeń i Tel Aviv mają po ~7 mln, ale popyt tam jest niemieckojęzyczny i hebrajskojęzyczny.

### Cztery wnioski, które zmieniają decyzję

**1. Integracje nie istnieją poza USA — a to połowa obecnej kampanii.**

Klaster integracyjny: USA **1 840**, UK 220, każdy inny rynek 50–80. Grupa `Integrations - API & ERP` **nie da się nigdzie przenieść**. Ekspansja geo = zmiana linii usługowej na Shopify albo mobile, nie tylko zmiana lokalizacji. To najważniejsze zdanie w tej sekcji.

**2. Granica „ograniczony budżetem" vs „ograniczony popytem" leży między 5. i 6. pozycją.**

Pozycje 1–5: więcej ruchu, niż zdołamy kupić — dosypanie budżetu daje więcej klików. Od 6. w dół: Holandia przy €4/klik wyczerpie się na ~90 klikach/mies. **niezależnie od budżetu**. Ta sama arytmetyka, która wcześniej zabiła pomysł kampanii tylko na Miami DMA (14.09) i tylko na Warszawę.

**3. Każdy rynek musi być osobną kampanią.** Dorzucenie UK/ZEA jako lokalizacji do istniejącej kampanii sprawi, że Maximize clicks znajdzie najtańszą kieszeń (`software house` w Arabii za €0.97) i wrzuci tam cały budżet — dokładnie mechanizm z incydentu FANUC (patrz audyt 15.09).

**4. Trendy się rozjeżdżają.** UK ma prawie wszystko na minusie r/r (−18% do −78%). Rosną: ZEA mobile +22%, Arabia +24%, Singapur custom dev +150%, Australia Shopify +23%. W horyzoncie 12-miesięcznym kolejność 2–5 wygląda inaczej niż w kwartalnym.

### Ograniczenie operacyjne: obsługa klienta tylko EN lub AR

Ustalenie Cezarego (15.09): leady da się obsłużyć **wyłącznie po angielsku lub arabsku** (arabski — Mohammad). To wycina z rankingu Polskę (#10) i Holandię (#6, i tak ograniczoną popytem), a **legitymizuje Zatokę** (#5 i #7), która bez arabskiej obsługi byłaby nie do wzięcia.

Hebrajskie landingi (`/he/...`) istnieją w `marotino_www_astro1`, ale bez obsługi po hebrajsku są bezużyteczne — strata zerowa, bo Tel Aviv i tak miał ~90 wyszukiwań.

### Zweryfikowane: arabska ścieżka jest gotowa end-to-end

Sprawdzone `curl`em 15.09, nie założone:

- `/ar`, `/ar/contact`, `/ar/services/mobile-app-development`, `/ar/services/e-commerce-solutions`, `/ar/services/custom-software-development` → wszystkie **200**.
- Strony mają poprawne `lang="ar"` i **`dir="rtl"`** — to realna lokalizacja, nie maszynowa zaślepka.
- `/ar/contact` ma **ten sam formularz Netlify `name="contact"`** co wersja angielska.
- **`GTM-5JRBQF9N` ładuje się na stronach arabskich.**

Wniosek: conversion action **„Marotino Lead - Contact Form" zadziała na ruchu arabskim bez żadnej nowej konfiguracji** — ten sam kontrakt `generate_lead` + `form_name: contact`. To eliminuje ryzyko powtórzenia historii z Xenią, gdzie tracking był pięć razy „naprawiony" przed wykryciem prawdziwej przyczyny.

Uwaga: `/ar/services/mobile-app-development` **nie ma własnego formularza** — CTA prowadzi do `/ar/contact`. Przy kampanii mobile w ZEA final URL powinien to uwzględniać (albo landing dostaje formularz, albo mierzymy konwersję po przejściu na `/ar/contact`).

### Decyzja: co dobrać

**Najpierw nic.** USA ma 17 500 wyszukiwań, a my kupujemy 3,5 klika dziennie — łapiemy błąd zaokrąglenia. Ale mamy **40 klików i zero leadów**, a repo samo zakłada 1–2 leady/mies. i „nie oceniać przed ~50 klikami". Nie wiemy więc, czy lejek konwertuje — a to **te same reklamy, landingi i oferta w każdym nowym rynku**. Jeśli USA nie konwertuje, Kanada ani ZEA tego nie naprawią. Dowieźć USA do ~100 klików, potem wybierać.

**Typ 1 — Kanada (bezpieczny, zero produkcji).** 1 620 wyszukiwań, wszystkie frazy Low competition, wejście €2.54 (najtaniej w świecie zachodnim), Shopify 480 + mobile 260. Te same reklamy, landingi i tracking; nakładające się godziny pracy z USA. Włączenie w godzinę, wyłączenie w dzień. **Wada uczciwie:** to „USA lite" — nie dywersyfikuje ryzyka, tylko zwiększa zasięg tej samej hipotezy.

**Typ 2 — ZEA (asymetryczny zakład, jedyny z realną przewagą).** Jedyny rynek, gdzie mamy coś, czego konkurencja nie ma: Mohammada i gotowe arabskie landingi z działającym trackingiem. `mobile app development company` 1 000 wyszukiwań, Low competition, €7.10, **+22% r/r** — jedyny duży rosnący wolumen w całym badaniu. **Wady:** brak grupy reklam pod mobile i brak arabskich tekstów reklam; Dubaj przepełniony agencjami; Low competition przy 1 000 wyszukiwań jest podejrzanie dobre i może oznaczać ruch niskiej jakości; Mohammad zostaje single point of failure na obsługę AR.

**Przy wymuszeniu jednego wyboru: Kanada** — koszt produkcji zero, decyzja odwracalna. ZEA to nie rozszerzenie geo, a osobny projekt (własna kampania, własny budżet, arabskie reklamy, grupa mobile, sprawdzony landing).

### Pułapki Keyword Plannera (uzupełnienie do wpisu z 14.09)

- **Lokalizacja wraca na Cypr przy KAŻDYM nowym planie**, nie raz na sesję. Potwierdzone czterokrotnie 15.09. Sprawdzać przed odczytem każdej tabeli.
- Dodanie lokalizacji **nie usuwa Cypru** — trzeba go ręcznie zdjąć, inaczej dane są sumą dwóch rynków i wyglądają wiarygodnie, mimo że są bezużyteczne.
- Selektor języka bywa **zablokowany na „All languages"** i nie da się go zmienić — przy frazach jednojęzycznych to nie problem, ale nie jest to formalna filtracja po języku.
- Przycisk usuwania lokalizacji to `<i>` z `aria-label="Remove targeted location, ..."`, nie `<button>` — skrypt szukający przycisków go nie znajdzie.
- Chip lokalizacji otwierający dialog to `div.location-button` — stabilniejszy selektor niż szukanie po nazwie kraju.

---

## Druga fala zmian 15.09.2026 — korekta zakresu usług i przebudowa pod €12/dzień

Kontekst: Cezary potwierdził **sufit budżetu €12/dzień jako nieprzekraczalny** oraz doprecyzował listę realnych kompetencji. To wywróciło część założeń z audytu porannego.

### Korekta kompetencji — Salesforce wypada

**Marotino NIE robi Salesforce.** Frazy salesforce'owe, które dodałem w nocy (`[salesforce integration services]`, `"salesforce integration services"`), zostały **usunięte**, a `salesforce` i `mulesoft` (platforma integracyjna Salesforce) dodane jako **negatywy**. Dodatkowo `step by step` — blokuje zapytania tutorialowe widziane w search terms.

Negatywy: 78 → **81**. Słowa kluczowe: 40 → 38.

Z reklamy grupy 1 wycięty nagłówek „Salesforce Integration" i opis wymieniający Salesforce. **Reklamowanie usługi, której nie dowozimy, jest gorsze niż spalony klik** — to zasada, nie kosmetyka.

**Wniosek proceduralny:** frazy dobierane wyłącznie z danych o wolumenie, bez weryfikacji z listą realnych kompetencji, to ten sam błąd co FANUC — tylko z drugiej strony. Wolumen mówi, czego ludzie szukają; nie mówi, czy umiemy to dowieźć. **Przed dodaniem frazy z nazwą produktu/vendora trzeba potwierdzić kompetencję.**

**Potwierdzone i wycięte (15.09, później tego samego dnia):** Marotino **nie robi też HubSpota ani QuickBooks**. Usunięte 3 frazy (`[hubspot integration services]`, `"hubspot integration services"`, `[quickbooks integration services]`), dodane negatywy `hubspot` i `quickbooks`. Negatywy: 81 → **83**. Słowa kluczowe: 55 → **52**.

Reklama grupy 1 zweryfikowana po zmianach — **zero wystąpień Salesforce, HubSpot i QuickBooks** w 14 nagłówkach i 3 opisach.

**Reguła do stosowania przy każdej kolejnej rozbudowie:** nazwa vendora w słowie kluczowym lub nagłówku wymaga potwierdzenia kompetencji, zanim trafi do konta. W ciągu jednego dnia trzy razy dobrałem frazy z danych o wolumenie, które okazały się usługami spoza oferty (Salesforce, HubSpot, QuickBooks). Wolumen mówi, czego szukają ludzie — nie mówi, co umiemy dowieźć.

### Zweryfikowane kompetencje i ich wolumen (USA)

| kompetencja | fraza | wolumen | konkurencja | wejście | werdykt |
|---|---|---|---|---|---|
| **Java / Spring Boot** | hire java developer | **390** | Low | **€3.57** | ✅ dodane, nowa grupa |
| | java development company | 210 | Low | — | ✅ |
| | java development services | 210 | Low | €5.61 | ✅ |
| | java software development services | 90 | Low | — | ✅ |
| | spring boot developer | 40 | Low | — | ✅ |
| **PrestaShop → Shopify** | prestashop to shopify migration | 20 | Low | **€2.96** | ✅ dodane |
| | migrate prestashop to shopify | 20 | Low | €4.07 | ✅ |
| **Shopify → Medusa** | shopify to medusa | **brak danych** | — | — | ❌ rynek nie istnieje |
| | medusa js development | brak danych | — | — | ❌ |
| **Coupa** | coupa punchout integration | **brak danych** | — | — | ❌ tylko outbound |
| | punchout catalog integration | 10 | Low | — | ❌ |

Java to **~960 wyszukiwań miesięcznie przy Low competition i wejściu €3.57** — tańsze niż nasza obecna średnia €3.61 i większa pula niż cały klaster integracyjny. Najlepszy stosunek wolumenu do ceny w całym dotychczasowym researchu przy potwierdzonej kompetencji.

Medusa i Coupa potwierdzają regułę z 14.09: **posiadanie mocnej kompetencji nie znaczy, że ktoś jej szuka.** Materiał na outbound, nie na Search.

### MVP / platformy / marketplace — sprawdzone, odrzucone

| fraza | wolumen | wejście | werdykt |
|---|---|---|---|
| mvp development services | 590 | €21.24 | ❌ za drogo przy €12/dzień |
| mvp development company | 480 | €25.96 (**+238% kwartalnie**) | ❌ za drogo, ale obserwować |
| saas mvp development | 70 | €33.38 | ❌ |
| **saas development company** | **590** | **€10.35** | ⚠️ najlepsze wejście w tym obszarze |
| saas platform development | 140 | — | ⚠️ |
| marketplace development company | 50 | €6.05 | ❌ |
| build a marketplace | 40 | €4.00 | ❌ intencja informacyjna |
| pozostałe marketplace | 10–20 | — | ❌ |

**Marketplace nie istnieje jako rynek wyszukiwań** (~160/mies. łącznie). MVP ma wolumen, ale przy €21–33 za klik i budżecie €12/dzień to jedno kliknięcie co drugi dzień. `mvp development company` rosnące +238% kwartalnie warto obserwować — jeśli budżet kiedyś wzrośnie, to pierwszy kandydat.

`saas development company` (590, Low, €10.35) to **właściwe wejście w „budowę platform"** — ten sam wolumen co MVP, połowa ceny, niższa konkurencja. Nie dodane z powodu budżetu.

### Nowa grupa reklam: Java & Spring Boot

12 słów (exact + phrase), landing `/services/custom-software-development`, ścieżka wyświetlania `marotino.com/Java/Spring-Boot`, RSA z 14 nagłówkami i 4 opisami. **Ad strength: Excellent.**

Google przy tworzeniu grupy **podstawia 15 własnych nagłówków AI** — i znów były to te same bzdury co przy Xenii: „Tired of Off-the-Shelf", „Agile. Transparent. Brilliant", „High-Quality Services", „Bespoke Systems Designed". Wszystkie podmienione na frazy dosłowne + wyróżniki (fixed-price, własność kodu, darmowa rozmowa 30 min).

Stan kampanii: **3 grupy, 55 słów kluczowych** (19 integracje / 12 Java / 24 Shopify).

**Pułapka (powtórka z 14.09):** przy zapisie grupy Google wyrzucił **„Confirm it's you"** i **cała wypełniona formatka przepadła** — grupa nie powstała mimo komunikatu sugerującego zapis. Po ręcznym potwierdzeniu przez Cezarego trzeba było zbudować wszystko od zera. **Zawsze weryfikować listę grup po tej blokadzie, nie ufać komunikatom.**

### Harmonogram reklam — zmiana decyzji wobec audytu porannego

Rano odradzałem harmonogram, bo kampania była **ograniczona zasięgiem** (2 serwujące słowa, 19 impresji). Po zmianach sytuacja się odwróciła: 7 serwujących słów i **wydatek €14.45 przy budżecie €12** — kampania jest teraz **ograniczona budżetem**.

Gdy wąskim gardłem jest budżet, rozkładanie go na 24h to marnotrawstwo. Ustawione: **wszystkie dni, 16:00–24:00 czasu konta (GMT+3) = 9:00–17:00 ET**, czyli pełny dzień pracy na wschodnim wybrzeżu USA.

Google informuje przy tym w UI: *„campaigns now pace toward a full month, distributed across your active ad schedule"* — czyli zawężenie godzin **nie zmniejsza wydatku miesięcznego**, tylko go koncentruje. Dokładnie o to chodziło.

**Nie dokończone:** druga linia harmonogramu 00:00–02:00 (= 17:00–19:00 ET, popołudnie zachodniego wybrzeża). UI uparcie stosował wybór godziny do pierwszego pola. Do dodania ręcznie, jeśli raport geo pokaże ruch z Zachodniego Wybrzeża.

**Pułapka UI:** listy godzin w edytorze harmonogramu **wiążą się z ostatnio realnie sfokusowanym polem**, nie z tym, na które wskazuje skrypt. Syntetyczny `.click()` na opcji trafia w niewłaściwy combobox. Konieczne prawdziwe zdarzenia CDP zarówno na polu, jak i na opcji z listy.

### Rewizja planu: przy €12/dzień NIE dzielimy na kampanie

Audyt poranny proponował 4 kampanie per linia usługowa z osobnymi budżetami. **Przy sufitze €12/dzień to jest błąd** — cztery kampanie po €3/dzień to cztery kampanie, które nie serwują.

Zostaje: **jedna kampania, trzy grupy, wspólny budżet, Maximize clicks.** Algorytm sam koncentruje wydatek na najtańszej puli — i to jest właściwe zachowanie, **pod warunkiem że ta pula jest dobra**. Przed 15.09 najtańszą kieszenią był FANUC; po negatywach są zapytania o API SaaS-owe i Java.

Rozbicie na kampanie per linia/rynek ma sens dopiero od ~€40/dzień.

### Arytmetyka sufitu — zapisana, żeby nie wracać do tematu

€360/mies. ÷ €3,61 = **~100 kliknięć miesięcznie**. Przy realnej konwersji B2B 2–4% to **2–4 leady miesięcznie**, koszt leada €90–180.

**Przy tym budżecie Google Ads nie jest maszyną do leadów, tylko tanim kanałem uzupełniającym.** Realnym źródłem leadów zostaje outbound (Twenty CRM, Apollo) — repo mówi to od 14.09 i dane tego nie podważyły. Software house zamyka projekty za €20–50k, więc jeden zamknięty deal pokrywa ten budżet na lata — ale oczekiwanie „maszyny" przy €12/dzień jest oczekiwaniem wbrew arytmetyce.

**Konsekwencja praktyczna:** skoro budżetu nie ruszamy, liczą się już tylko dźwignie, które nie kosztują mediów:

1. **Pętla konwersji do Twenty CRM** — `uploadClickConversions` jest napisane i **nigdy nie przetestowane**. Przy 100 klikach miesięcznie algorytm ma mało danych, więc tym bardziej muszą być prawdziwe, a nie „ktoś wysłał formularz".
2. **Telefon w formularzu `/contact`** — obecnie imię, email, wiadomość. Dla B2B zamykanego rozmową to największa pojedyncza strata w lejku.
3. **Dedykowane landingi zamiast `/contact`** — konwertują 2–3× lepiej. Przy 100 klikach to różnica między 2 a 5 leadami, czyli więcej niż dałoby podwojenie budżetu.

Punkty 1–3 są poza Google Ads i mogą dać większy przyrost leadów niż jakakolwiek zmiana w kampanii. **Przy zablokowanym budżecie gra przenosi się na stronę.**

### Zostało do zrobienia (aktualizacja)

- [ ] Potwierdzić, czy umiemy HubSpot i QuickBooks — jeśli nie, usunąć frazy jak salesforce'owe
- [ ] **Korekta stawek na urządzenia** — obcięcie mobile (B2B konwertuje na desktopie); nie ustawione, brak danych, do zrobienia po pierwszych 50 klikach
- [ ] Druga linia harmonogramu 00:00–02:00 pod zachodnie wybrzeże USA
- [ ] `uploadClickConversions` na żywo → pętla Twenty CRM
- [ ] Telefon w formularzu `/contact`, dedykowane landingi per linia
- [ ] Zmierzyć popyt na **AI agents / chatboty** — to realny produkt powtarzalny Marotino (10 wdrożeń Chatwoot+Dify w 2026), a nigdy nie sprawdzony w Keyword Plannerze
- [ ] Zmierzyć Flutter / React Native
- [ ] Przegląd search terms po tygodniu — dosypać negatywy na zapytania informacyjne (`creating api in java`, nazwy firm typu `systems integration inc sii`)
- [ ] Advertiser verification przed **2026-10-07** — konto pokazuje już ostrzeżenie „Some ads may be limited"

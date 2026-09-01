# Google Ads — Xenia (Marotino)

Dokumentacja pierwszej kampanii Google Ads dla produktu **Xenia** (AI recepcjonista dla hoteli, `marotino.com/products/xenia`). Ten plik jest napisany tak, żeby ktoś (człowiek albo agent) mógł zrozumieć od zera co jest ustawione, dlaczego, i na co uważać — bez odtwarzania całego procesu decyzyjnego.

Uruchomiona: **30.08.2026**.

---

## TL;DR — stan na dziś

- Konto Google Ads: **846-403-8850**, nazwa konta "Marotino", waluta **EUR**, kraj **Cyprus**.
- Kampania: **"Xenia - Hotel AI Receptionist - FL Search"**, typ **Search only** (bez Display/PMax — brak assetów graficznych na start).
- Geo: **Floryda, USA** (nie global, nie Polska).
- Budżet: **€20/dzień** (świadomy smoke test, nie docelowy budżet).
- Cel konwersji: **Submit lead form** — realny formularz na stronie (`xenia-pilot`, Netlify Forms), nie telefon.
- Billing: profil płatności **Marotino CY LTD** (Cyprus, VAT CY60017620T), Postpay, karta Visa …9422.
- Status: **Enabled**, w fazie "Bid strategy learning" (normalne pierwsze dni).

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

## Do zrobienia / do obserwowania

- [ ] Sprawdzić za kilka godzin czy status kampanii zmienił się z "Eligible (Misconfigured)" na normalny "Eligible" i czy zaczęły się impressions.
- [ ] Po pierwszym tygodniu: sprawdzić czy geo Floryda faktycznie łapie relewantny ruch, czy trzeba rozszerzyć/zawęzić.
- [ ] Rozważyć banery/PMax jako kampanię równoległą, jeśli powstaną assety graficzne.
- [ ] Zdecydować docelowy budżet dzienny po zobaczeniu realnego CPC/CPA z pierwszego tygodnia (start: €20/dzień to smoke test, nie budżet docelowy).
- [ ] Ustawić alert/przegląd tygodniowy leadów z formularza `xenia-pilot` (Netlify Forms dashboard) vs conversions w Google Ads — porównać czy się zgadzają.
- [ ] Po zebraniu pierwszych realnych konwersji z "Xenia Lead Submitted": wrócić z bid strategy na "Maximize conversions" (patrz incydent 31.08.2026 wyżej).
- [ ] Rozważyć usunięcie/wyłączenie martwej akcji "Form" (Secondary) po potwierdzeniu że nowa akcja działa — żeby nie zaśmiecać listy conversion actions.

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

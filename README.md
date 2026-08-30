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

## Pułapki podczas zakładania konta (żeby nie powtórzyć)

1. **Konto agencyjne z wieloma klientami pod jednym loginem** (ostrowski@marotino.com) — na liście kont Ads są zawieszone/w budowie drafty innych klientów (Optienergia, Batycki, Tincors, Menusso, itd.). Kreator "Create your first campaign" **domyślnie podłącza się do ostatniego niedokończonego draftu** — jeden z nich (konto 838-841-9692) miał wpisane dane **Optienergia** zamiast Marotino. Zawsze sprawdzić przy starcie nowej kampanii, czy nie kontynuujemy cudzego draftu — użyć przycisku "New Google Ads Account" → "Create a new account", nie "Finish setting up".
2. **Waluta konta domyślnie ustawiła się na PLN** mimo że Marotino CY LTD rozlicza się w EUR — trzeba było ręcznie zmienić w kroku budżetu (dropdown przy polu kwoty). **Kraj konta ("Germany" domyślnie, źle wykryty) też trzeba było poprawić na Cyprus** — dopiero po tej poprawce Google Ads pokazał już istniejący profil płatności Marotino CY LTD (wcześniej sugerował założenie nowego z danymi osobistymi z konta Google).
3. **Profil płatności Marotino CY LTD JUŻ ISTNIEJE** (ID: 0708-1259-6138, współdzielony z innymi usługami Google Ads/Cloud) — nie trzeba zakładać nowego. Dane: VAT CY60017620T, adres Evripidou 9A, 3031 Limassol, Cyprus.
4. **Pierwsza autoryzacja karty (Visa …9422) nie powiodła się** przy pierwszej próbie (bank oznaczył jako podejrzaną — typowe przy pierwszej płatności zagranicznej/online). Zadziałało po odblokowaniu karty przez użytkownika i dodaniu jej ponownie.
5. Krok płatności wymaga **"Verify it's you"** — weryfikacja tożsamości w osobnym oknie popup, którą musi przejść człowiek (2FA), nie da się zautomatyzować.

## Do zrobienia / do obserwowania

- [ ] Sprawdzić po ~3h czy conversion action "Form" (Website) przeszła weryfikację.
- [ ] Po pierwszym tygodniu: sprawdzić czy geo Floryda faktycznie łapie relewantny ruch, czy trzeba rozszerzyć/zawęzić.
- [ ] Rozważyć banery/PMax jako kampanię równoległą, jeśli powstaną assety graficzne.
- [ ] Zdecydować docelowy budżet dzienny po zobaczeniu realnego CPC/CPA z pierwszego tygodnia (start: €20/dzień to smoke test, nie budżet docelowy).
- [ ] Ustawić alert/przegląd tygodniowy leadów z formularza `xenia-pilot` (Netlify Forms dashboard) vs conversions w Google Ads — porównać czy się zgadzają.

#  Olist Store – Analisi vendite e performance con Power BI

##  Introduzione

Questo progetto ha l’obiettivo di esplorare e visualizzare i dati di Olist Store, una piattaforma di e-commerce brasiliana che mette in contatto venditori e clienti in tutto il Paese.
Attraverso Power BI è stato realizzato un *report interattivo* per monitorare l’andamento degli ordini, i ricavi e confrontare le performance nel tempo.

Il focus principale è su:

*  dinamica temporale degli ordini e dei ricavi;
*  analisi comparativa anno su anno;
*  possibilità di filtrare e segmentare i dati in base a stato dell’ordine e dimensione geografica.

---

##  Dataset utilizzati

I dati sono stati forniti in formato .csv. Tra i nove file originali, ne sono stati selezionati cinque per costruire il modello dati necessario alle analisi:

| *Nome file*                 | *Contenuto*                                       | *Campi chiave*                                 |
| ----------------------------- | --------------------------------------------------- | ------------------------------------------------ |
| olist_orders_dataset        | Ordini e relative informazioni temporali e di stato | order_id, order_status, order_purchase_timestamp |
| olist_order_items_dataset   | Dettagli degli articoli venduti                     | order_id, product_id, price, freight_value       |
| olist_products_dataset      | Informazioni sui prodotti                           | product_id, product_category_name                |
| olist_order_reviews_dataset | Recensioni dei clienti                              | review_id, review_score, review_creation_date    |
| olist_customers_dataset     | Informazioni geografiche sui clienti                | customer_id, customer_state, customer_city       |

---

##  Modellazione e trasformazione dei dati

###  Pulizia e preparazione in Power Query

* Controllo e uniformazione dei *tipi colonna* (testo, numerico, data, valuta).
* Gestione dei *valori nulli e inconsistenze*.
* Eliminazione dei campi superflui per ottimizzare la dimensione complessiva.

###  Modello dati

È stato implementato un *modello a stella* (Star Schema), con olist_orders_dataset come tabella dei fatti principale.
Sono state create due *tabelle calendario* (per ordini e per recensioni) e stabilite relazioni coerenti per supportare analisi temporali e confronti.

Relazioni principali:

* 1→molti tra olist_orders_dataset e olist_order_items_dataset
* molti→1 con tabella CALENDARIO per analisi temporali
* molti→1 con olist_products_dataset per dettaglio prodotto
* molti→1 con tabella ReviewCalendar per analisi recensioni

---

##  Misure DAX

| *Nome misura*     | *Formula*                                                                                                    | *Descrizione*                                  |
| ------------------- | -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| Q_Ordini          | COUNTROWS(olist_order_items_dataset)                                                                         | Numero totale di articoli ordinati               |
| Q_Ordini_anno_PY  | CALCULATE(COUNT(olist_order_items_dataset[order_id]), PARALLELPERIOD(CALENDARIO[Date], -12, MONTH))          | Ordini nello stesso periodo dell’anno precedente |
| Revenue_Totale    | SUMX(olist_order_items_dataset, olist_order_items_dataset[price] + olist_order_items_dataset[freight_value]) | Ricavi totali (prezzo + spedizione)              |
| Revenue_Totale_PY | CALCULATE([Revenue_Totale], PARALLELPERIOD(CALENDARIO[Date], -12, MONTH))                                    | Ricavi totali dell’anno precedente               |
| Var_%_Ordini      | DIVIDE([Q_Ordini]-[Q_Ordini_anno_PY],[Q_Ordini_anno_PY])                                                     | Variazione percentuale degli ordini              |
| Var_%_Ricavo      | DIVIDE([Revenue_Totale]-[Revenue_Totale_PY],[Revenue_Totale_PY])                                             | Variazione percentuale dei ricavi                |

---

##  Scelte di visualizzazione

* *Line chart* per analizzare i trend mensili di ordini e ricavi e confrontarli con l’anno precedente.
* *Mappa geografica* per rappresentare la distribuzione degli ordini per stato brasiliano.
* *Indicatori KPI* per evidenziare le variazioni YoY in modo immediato.
* *Filtri interattivi* (slicer) per esplorare i dati per anno, mese e stato ordine.

 La palette colori è stata ispirata al *brand Olist*, per garantire coerenza visiva e migliorare la leggibilità del report.

---

##  Approccio allo storytelling

Il report privilegia un’impostazione *descrittiva e interattiva*:

* Ogni visualizzazione fornisce informazioni oggettive e immediatamente leggibili;
* Non sono state aggiunte considerazioni strategiche o operative, *in quanto i trend emersi risultano già evidenti dai dati*;
* L’attenzione è stata posta sull’esperienza utente e sulla chiarezza nella lettura dei KPI principali.

---

##  Risultati

Il risultato è un report chiaro, navigabile e coerente con le best practice di data visualization.
L’utente finale può:

* confrontare ordini e ricavi mese per mese e anno su anno,
* analizzare la distribuzione geografica,
* estrarre insight senza bisogno di analisi aggiuntive.

---

 Autore: Stefano Maurizi
 Progetto realizzato con Power BI Desktop
 Dati: Olist Store – E-commerce Brasiliano

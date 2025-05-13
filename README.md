🧾 README - Progetto Previsione Premio Assicurativo
📌 Titolo del Progetto
"Insurance Premium Prediction System"
Un sistema predittivo intelligente per l’ottimizzazione dei premi assicurativi.

🎯 Obiettivo del Progetto
Sviluppare un sistema di machine learning capace di prevedere in modo accurato il premio assicurativo annuale per un cliente, sulla base del suo profilo anagrafico, comportamentale e storico.

💼 Valore per il Business
✅ Ottimizzazione della politica di pricing:
Evita premi troppo alti → perdita di competitività

Evita premi troppo bassi → perdita economica

✅ Profilazione del rischio più accurata:
Include variabili comportamentali come:

Abitudine al fumo

Frequenza di esercizio fisico

Feedback lasciato alla compagnia

✅ Riduzione del rischio finanziario:
Grazie a modelli predittivi avanzati e metriche affidabili (RMSLE)

🔧 Tecnologie Utilizzate
Strumento	Utilizzo
Python	Linguaggio principale
Pandas / NumPy	Gestione e trasformazione dei dati
Scikit-learn	Preprocessing, pipeline, validazione
LightGBM	Modello di regressione veloce e performante
Optuna	Ottimizzazione iperparametri
Matplotlib	Visualizzazione dei dati
Joblib	Salvataggio e caricamento dei modelli
CSV/Excel	Formato di input/output dei dati
PowerShell/CLI	Esecuzione script interattivo

🧠 Modello Predittivo
Tipo: Regressore (LGBMRegressor)

Target: Premium Amount → valore continuo

Metriche:

🟢 RMSLE train: ~1.04

🟡 RMSLE Kaggle: ~1.08 (competizione con test nascosto)

Validazione: Cross-Validation (KFold) su 5 fold

🗂️ Struttura del Progetto (Moduli Principali)
Modulo	Descrizione
01_preprocessing.py	Pulizia iniziale, gestione missing values
02_feature_engineering.py	Creazione di nuove variabili derivate (es. flag, binning, date parsing)
03_eda.py	Analisi esplorativa (statistiche, istogrammi, frequenze)
04_model_training.py	Training iniziale del modello con metriche di base
05_validation.py	Cross-validation + salvataggio modello più performante
06_hyperparameter_tuning.py	Tuning automatico con Optuna per trovare la combinazione ottimale
07_generate_submission.py	Predizione finale + creazione file submission.csv per competizione Kaggle
interactive_menu.py	Menù CLI per uso pratico del sistema (inserimento dati + predizione live)

📈 Visualizzazione e Supporto Commerciale
Output grafici esportati in PNG:

Istogrammi

Matrice di correlazione

File Excel e CSV per analisi:

Frequenze categorie

Statistiche numeriche

Performance modello salvate in baseline_scores.txt

Menù interattivo per demo dal vivo

Predizione premio a partire da input utente

⚠️ Principali Sfide Affrontate
Sfida	Soluzione adottata
Dataset con 1.2M di record	Pulizia e imputazione automatica + salvataggio in CSV ottimizzati
Elevata cardinalità categoriche	Target encoding personalizzato e ordinamento gestito
Overfitting	KFold + regolarizzazione + log-transform sul target
Incoerenze tra train/test	Uso di pipeline per allineare feature engineering
RMSLE su Kaggle più alto	Analisi approfondita del comportamento del test nascosto

🧪 Uso del Menù Interattivo
Comando:

bash
Copia
Modifica
python interactive_menu.py
Funzionalità disponibili:

Riepilogo statistico

Visualizzazione bins

Logica dell’indice HealthRisk

Performance del modello

Inserimento cliente per predizione reale

Visualizzazione grafici (EDA)

🧮 Logiche Proprietarie Implementate
HealthRisk_index: differenza tra livello di fumo e frequenza di esercizio

Feedback_length: metrica del sentiment implicito dal feedback

missing_flags: utili per rilevare dati parziali e stimare rischi nascosti

🚀 Espansioni Future
Integrazione con frontend web per demo commerciale

Uso di explainability tool (SHAP)

Integrazione con database assicurativo reale

Rilascio in ambiente cloud (es. AWS, GCP)tuning per migliorare il punteggio finale.

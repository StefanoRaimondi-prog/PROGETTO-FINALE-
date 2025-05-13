1. Caricamento e pulizia dei dati
Logica:

Lettura dei dataset train.csv e test.csv

Gestione dei valori mancanti con flag dedicati (*_missing_flag)
File: 01_preprocessing.py

2. Feature Engineering
Logica:

Creazione di nuove feature basate su data (Policy_Age_Days, Policy_Start_Year, ecc.)

Binning di età, credit score e vehicle age (Age_bin, CreditScore_bin, VehicleAge_bin)

Lunghezza del feedback cliente (Feedback_length)
File: 02_feature_engineering.py

3. Analisi esplorativa dei dati (EDA)
Logica:

Statistiche descrittive su numeriche e categoriche

Frequenze delle categorie principali

Correlazioni tra le variabili

Salvataggio di grafici e output per reporting
File: 03_eda.py

4. Calcolo dell’Health Risk Index
Logica:

Assegnazione di punteggi a comportamenti salutari:
HealthRisk_index = SMOKING_SCORE - EXERCISE_SCORE

Utile per inferire il rischio assicurativo legato allo stile di vita
File: 02_feature_engineering.py, visualizzato anche nel interactive_menu.py

5. Preprocessing dinamico
Logica:

Preprocessing delle variabili numeriche con SimpleImputer + StandardScaler

Preprocessing delle categoriche con SimpleImputer + OrdinalEncoder

Gestione dei dati tramite ColumnTransformer
File: 05_validation.py, 07_generate_submission.py

6. Cross-Validation (CV)
Logica:

Validazione del modello con KFold(n_splits=5)

Calcolo del punteggio RMSLE medio su 5 suddivisioni
File: 05_validation.py
Obiettivo: ottenere stime robuste delle performance

7. Modellazione con LightGBM
Logica:

Uso di LGBMRegressor per gestire problemi di regressione complessi e dataset ampi

Ottimizzazione iperparametri (in 06_hyperparameter_tuning.py)
File: 05_validation.py, 06_hyperparameter_tuning.py, 07_generate_submission.py

8. Salvataggio del modello e pipeline
Logica:

L’intero pipeline (preprocessor + regressor) salvato in un file .pkl
File: 05_validation.py
Uso successivo: riutilizzato per predizioni nel interactive_menu.py

9. Predizione interattiva per nuovi clienti
Logica:

Input utente → feature engineering in tempo reale

Passaggio dei dati nel modello già addestrato

Output: stima del premio assicurativo
File: interactive_menu.py

10. Menù CLI interattivo
Logica:

Interfaccia a riga di comando con più voci:

Riepilogo dataset

Logiche di calcolo

Performance del modello

Predizione nuova

Visualizzazione grafici
File: interactive_menu.py

11. Salvataggio e caricamento file esterni
Logica:

Esportazione CSV (numeric_stats.csv, correlation_matrix.csv)

Salvataggio output EDA (hist_x.png)

Caricamento modello (.pkl) e sample_submission
File: vari

12. Metrica RMSLE per regressione
Logica:

Uso di Root Mean Squared Logarithmic Error per premi assicurativi

Più adatta di RMSE per target asimmetrici e positivi
File: 05_validation.py, 07_generate_submission.py


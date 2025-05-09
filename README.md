# PROGETTO-FINALE-
# Progetto Finale Assicurazioni

Questo progetto affronta la competition Kaggle **Playground Series S4E12**, con l'obiettivo di predire il **Premium Amount** di polizze assicurative sui dati di train/test forniti, raggiungendo un **RMSLE ≤ 1.03**.

## Struttura e logica del progetto

Il flusso generale è suddiviso in 7 moduli Python, che vanno eseguiti in sequenza dall'estrazione delle feature fino alla creazione della submission:

1. **01\_feature\_temporali.py**

   * Carica `train.csv` e `test.csv`, parse della data `Policy Start Date`.
   * Estrae feature temporali:

     * `Policy_Start_Year`, `Policy_Start_Month`, `Policy_Start_DayOfWeek`
     * `Policy_Age_Days` (distanza dalla data di esecuzione)
   * Salva `*_with_temporal.csv`.

2. **02\_missing\_flags.py**

   * Carica i CSV con le feature temporali.
   * Per ogni colonna originale (numeriche e categoriche), crea un flag booleano `_missing_flag` che indica valori nulli.
   * Salva `*_with_missing_flags.csv`.

3. **03\_eda.py**

   * Esegue un’analisi esplorativa del dataset (`train_with_missing_flags.csv`):

     * Distribuzioni statistiche (istogrammi, violin plot) delle variabili numeriche.
     * Frequenze e counts delle categoriche.
     * Matrice di correlazione e scatter plot tra `Health Score`, `Credit Score`, `Annual Income` e `Premium Amount`.
   * Genera report e grafici in output (cartella `reports/`).

4. **04\_feature\_engineering.py**

   * Carica `*_with_missing_flags.csv`.
   * Binning numerico:

     * **Age\_bin**, **CreditScore\_bin**, **VehicleAge\_bin** con cuts predefiniti.
   * Composite feature **HealthRisk\_index** = mappatura di `Smoking Status` (0–2) meno mappatura di `Exercise Frequency` (0–3).
   * **Feedback\_length** dalla lunghezza testo di `Customer Feedback`.
   * Salva `train_fe.csv` e `test_fe.csv`.

5. **05\_validation\_and\_baseline.py**

   * Carica `train_fe.csv`.
   * Seleziona X/y, log1p trasforma `Premium Amount` per stabilità numerica.
   * Costruisce pipeline:

     * Imputer (median) + **StandardScaler** per variabili numeriche.
     * Imputer (constant) + **OrdinalEncoder** per categoriche.
   * Addestra un **LightGBMRegressor** di base su 5-fold CV, misura RMSLE per fold e media.
   * Fit finale su tutti i dati, salva pipeline in `baseline_lgbm_pipeline.pkl`.

6. **06\_hyperparameter\_tuning.py**

   * Carica `train_fe.csv` e preprocessa una volta.
   * Usa **Optuna** per la ricerca su 5-fold CV (trigger 5 trial rapidi):

     * Parametri campionati: `n_estimators`, `learning_rate`, `num_leaves`, `max_depth`, `min_data_in_leaf`, `lambda_l1`, `lambda_l2`, `feature_fraction`, `bagging_fraction`, `bagging_freq`.
   * Restituisce il **Best trial** (RMSLE + parametri) e salva in `best_lgbm_params.pkl`.

7. **07\_generate\_submission.py**

   * Carica `train_fe.csv`, `test_fe.csv`, i migliori parametri da Optuna e `sample_submission.csv`.
   * Ricostruisce la stessa pipeline di preprocessing + **LGBMRegressor** con `best_lgbm_params.pkl`.
   * Fit su tutti i dati di train (log1p target), predice su test, esponenziale inversa e clip ≥0.
   * Allinea le predizioni con gli ID via merge, salva `submission.csv` pronta per Kaggle.

---

## Strumenti e librerie utilizzate

* **Python 3.13**
* **Pandas** / **NumPy** per gestione dati e feature engineering
* **scikit-learn** per pipeline, preprocessing (Imputer, StandardScaler, OrdinalEncoder), K-Fold CV, metriche (RMSLE)
* **LightGBM** per il modello base e ottimizzato
* **Optuna** per hyperparameter tuning
* **Joblib** per serializzazione modelli e parametri
* **Matplotlib** / **Seaborn** (nel modulo EDA) per grafici esplorativi
* **CLI script** (`interactive_menu.py`) per un menu interattivo di esplorazione e predizione

## Come eseguire il progetto

1. Posizionarsi nella cartella del progetto con i file `train.csv`, `test.csv`, `sample_submission.csv`.
2. Eseguire in ordine i moduli:

   ```bash
   python 01_feature_temporali.py
   python 02_missing_flags.py
   python 03_eda.py    # opzionale, genera grafici
   python 04_feature_engineering.py
   python 05_validation_and_baseline.py
   python 06_hyperparameter_tuning.py
   python 07_generate_submission.py
   ```
3. Caricare `submission.csv` su Kaggle per ottenere il punteggio RMSLE.
4. (Opzionale) Avviare `python interactive_menu.py` per un’interfaccia CLI.

---

Con questa pipeline strutturata è facile iterare: si possono aggiungere nuove feature, provare modelli alternativi o raffinare ulteriormente l’hyperparameter tuning per migliorare il punteggio finale.

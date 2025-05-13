1. Rimozione o Sostituzione di Colonne
Colonne rimosse:

id: non informativa per la predizione, usata solo per unire risultati nella submission.

Premium Amount: rimossa da X perché è la variabile target.

Colonne sostituite o trasformate:

Policy Start Date: trasformata in Policy_Start_Year, Month, DayOfWeek, Policy_Age_Days.

Customer Feedback: da testo → in lunghezza con Feedback_length.

Variabili con valori mancanti → sostituite da *_missing_flag.

2. Gestione delle Colonne Numeriche
Trattate con:

SimpleImputer(strategy='median') per gestire i NaN.

StandardScaler() per normalizzare le scale.

Creazione di bin (pd.cut) per:

Age → Age_bin

Credit Score → CreditScore_bin

Vehicle Age → VehicleAge_bin

3. Gestione delle Colonne Categoriche (Stringhe)
Preprocessing con:

SimpleImputer(strategy='constant', fill_value='missing') per i NaN.

OrdinalEncoder(handle_unknown='use_encoded_value', unknown_value=-1) per convertire in numeri.

Alcune categorie con logica specifica:

Smoking Status + Exercise Frequency → trasformate in numeri con dizionari (SMOKE_MAP, EXERCISE_MAP) per calcolare HealthRisk_index.

4. Altre Trasformazioni Importanti
Log transformation sul target:

y = np.log1p(Premium Amount) per stabilizzare la varianza e migliorare la performance.

Inverse transformation per le predizioni:

y_pred = np.expm1(model.predict(X)) per tornare alla scala originale del premio.

In sintesi:
Operazione	Colonne Interessate
Rimozione	id, Premium Amount (da X)
Estrazione data	Policy Start Date → Anno, Mese, Giorno, Età polizza
Imputazione mancanti	Tutte le numeriche + categoriche
Binning	Età, Credit Score, Età veicolo
Stringhe → numeri	OrdinalEncoder, mapping fumatori/esercizio
Flag mancanti	Tutte le variabili input
Feature engineering nuove	HealthRisk_index, Feedback_length

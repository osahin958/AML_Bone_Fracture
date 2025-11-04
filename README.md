# Knochenfraktur-Erkennung

Workflow zur Ausführung mit originalem und augmentiertem Datensatz.

---

## 1. Setup

```
Installiere Python 3.8 oder neuer.
- Empfehlung: Verwende eine virtuelle Umgebung 
- Benötigte Pakete installieren:

pip install -r requirements.txt
```

---

## 2. Datensatzstruktur

```text
BoneFracturesDetection/
  data.yaml
  train/images/*.jpg
  train/labels/*.txt
  valid/images/*.jpg
  valid/labels/*.txt
  test/images/*.jpg
  test/labels/*.txt
```

Stelle sicher, dass `data.yaml` Pfade und Klassenliste (`names`) korrekt enthält.

---

## 3. Ablaufübersicht

1. Baseline-Training mit Originaldaten (`BoneFractureDetection.ipynb`)
2. Augmentation erzeugen (`DataAugmentation.ipynb`)
3. Re-Training mit augmentierten Daten (`BoneFractureDetection.ipynb` erneut)
4. Ergebnisse vergleichen und dokumentieren

---

## 4. Baseline: Originaler Datensatz

Notebook: `BoneFractureDetection.ipynb`

Schritte:

1. Imports & Datenprüfung
2. EDA: Klassenverteilung sichern (Screenshot)
3. Training (Run-Name z.B. `baseline_run`)
4. Evaluation: `model.val(split='test')` → mAP50 / mAP50-95 / Konfusionsmatrix
5. Beispielvorhersagen unter `runs/detect/baseline_run/predictions/`

Ergebnisse liegen unter `runs/detect/baseline_run/`.

---

## 5. Augmentation erzeugen

Notebook: `DataAugmentation.ipynb`

Ziel: Unterrepräsentierte Klassen (Segmental, Linear, Oblique ...) synthetisch erhöhen.

Schritte:

1. Augmentationsplan prüfen
2. Ausführen → neue Dateien mit Präfix `aug_` in `train/images` & passende Labels in `train/labels`
3. Kontrolle: Anzahl neuer Dateien
4. Optional: EDA erneut für neue Verteilung

---

## 6. Re-Training (augmentierte Daten)

Notebook wieder: `BoneFractureDetection.ipynb`

Schritte:

1. Prüfen ob `aug_` Dateien vorhanden sind
2. Neuen Run-Namen wählen: `augmented_run`
3. Training starten (ggf. Epochen +5–10 erhöhen)
4. Evaluation erneut auf Test-Set (`split='test'`)
5. Konfusionsmatrix baseline vs. augmented vergleichen

Ziel: Mehr korrekte Treffer für seltene Klassen, weniger False Negatives.

---

## 7. Bereinigung / Zurücksetzen

In DataAugmentation.ipynb 2. Code Feld ausführen.

---

Viel Erfolg!

# Biometric Identification and Face Recognition System

This repository contains an end-to-end facial recognition pipeline designed to run within the **Google Colab** ecosystem. The architecture integrates deep learning models (`ArcFace`, `RetinaFace`, and `MiniFASNet`) to deploy a secure, real-time framework capable of executing user enrollment, open-set identification, 1:1 verification, and presentation attack detection (anti-spoofing).

---

## 📐 System Architecture & Module Details

The framework is segmented into two sequential workflows across the operational notebooks:

### 1. Automated Feature Extraction 

* **Detection & Gating:** It cycles through image directories, isolates faces via `RetinaFace`, and normalizes the input.
* **Feature Representation:** Leverages `DeepFace` with the **ArcFace** model backend to extract a 512-dimensional vector mapping unique facial traits.
* **Serialization:** Serializes and exports the final feature maps into structured binary storage.

### 2. Core Real-Time Engine
Deploys a live interactive interface inside a `Gradio` Web Dashboard, enforcing a strict structural pipeline before executing face-matching routines:
* **Strict Quality Check:**
  * *Resolution Check:* Discards frames falling below standard HD specifications.
  * *Aspect Ratio Gating:* Validates a rigid Portrait 16:9 aspect ratio.
  * *Orientation Filter:* Immediately flags and blocks horizontal captures.
  * *Illumination Guard:* Computes average pixel intensity, discarding under-exposed or over-exposed frames.
  * *Defocus Blur Guard:* Employs the Laplacian Variance algorithm on downsampled grids.
  * *Crowd Gate:* Restricts matching protocols if the detector backend extracts 0 faces or more than 1 face.
* **Liveness Check:**
  Clones and mounts the **Silent-Face-Anti-Spoofing** repository into the kernel environment. 
* **Biometric Decision Management:**
  * *Deduplication Gate:* Rejects new enrollments if the computed cosine distance against any pre-existing database matrix drops below a security threshold, preventing identity duplication.
  * *1:1 Verification:* Evaluates target probes specifically against a target `claimed_id` vector profile using an operational decision barrier.
  * *1:N Open-Set Identification:* Runs a nearest-neighbor sweep across the persistent space, enforcing a matching threshold. It executes an active **Reject Option** ("Sconosciuto") to block unauthorized intrusions.

---
## 🛠️ Project File 

* **`Sistema_di_Riconoscimento.ipynb`**: Primary application notebook housing the cascade logic functions.
* **`Estrazione_feature_gallery.ipynb`**: Pipeline setup script that iterates across samples to build the finalized ledger.

---

<img width="666" height="258" alt="Screenshot 2026-07-01 alle 15 02 48" src="https://github.com/user-attachments/assets/20cb296f-5ee4-43ec-8663-5832a57a04dd" />

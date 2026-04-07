# ToyinTriageAI

> **A Bayesian clinical decision-support PWA for Nigerian Primary Health Centres — offline-first, zero-dependency, built for the realities of the last mile.**

🔗 **Live Demo:** [https://onwunaru760-a11y.github.io/triageAI](https://onwunaru760-a11y.github.io/triageAI)

---

## The Problem

Nigerian Primary Health Centres (PHCs) handle complex, high-acuity presentations with limited staffing, unreliable internet, and no specialist backup. A community health officer seeing a patient with chest pain at 11pm — with no cardiologist, no troponin test, and no reliable Wi-Fi — has to triage that patient on clinical judgment alone.

Judgment can be augmented. This tool does that.

---

## What It Does

NaijaTriageAI is a two-module Progressive Web App providing structured, Bayesian triage support for two of the most high-stakes clinical scenarios in Nigerian PHC settings:

### Module 1: Chest Pain Triage (`index.html`)
Walks the clinician through a structured symptom assessment and computes a differential probability across the major causes of chest pain — calibrated with Nigerian-specific priors:

- **Sickle Cell Disease** (higher baseline prevalence in Nigerian population)
- **HIV-related cardiac complications** (adjusted for national HIV prevalence)
- **Rheumatic Heart Disease** (calibrated for post-streptococcal sequelae rates)
- **Delayed presentation bias** (patients who present late have systematically different probabilities)
- Musculoskeletal, pleuritic, GERD, and other common differentials

### Module 2: Maternal & Neonatal Health (`mnh.html`)
Structured triage and decision support for two obstetric/neonatal emergencies:

- **Postpartum Haemorrhage (PPH):** Step-by-step severity assessment with blood loss estimation, uterotonic decision tree, and escalation criteria
- **Neonatal Asphyxia:** APGAR-informed assessment with resuscitation prompts calibrated for resource-limited settings (bag-valve-mask availability, warming, timing)

---

## Why Bayesian

Bayesian inference is the right framework for clinical triage because it mirrors how experienced clinicians actually think — not "does this patient have X?" but "given everything I know about this patient and this population, how likely is X compared to Y?"

The priors in this tool are not defaults from a US textbook. They reflect:

- Nigeria's SCD carrier frequency (~24% trait, ~1–2% SS disease)
- Sub-Saharan HIV prevalence adjusted to Nigerian epidemiology  
- Rheumatic heart disease rates in populations with historically inadequate strep treatment
- Maternal mortality patterns from Nigerian PHC data

Changing the priors for a different population is straightforward — the logic is transparent and documented.

---

## Architecture

```
triageAI/
├── index.html    ← Chest Pain Triage Module (fully self-contained PWA)
└── mnh.html      ← Maternal & Neonatal Health Triage Module (fully self-contained PWA)
```

No build step. No npm. No server required.

Each file is a **complete, self-contained application** — HTML, CSS, JavaScript, and Bayesian logic in a single file. This is a deliberate architectural choice:

- **Works offline** the moment you open it in a browser
- **Installable as a PWA** on Android/iOS home screens via browser prompt
- **No backend dependency** for core triage function (connectivity is unreliable in the settings this tool targets)
- **Background sync** to Flask/Supabase backend (deployed on Render) when connectivity is available — for audit trails and aggregate data collection

---

## Backend Sync (Optional)

A lightweight Flask API handles background data sync when the device reconnects. The `/health` endpoint confirms the API is live.

| Endpoint | Function |
|----------|----------|
| `GET /health` | Health check — returns `{"status": "ok"}` |
| `POST /sync` | Receives queued triage sessions from offline PWA |

The backend is intentionally decoupled — the PWA functions fully without it. Sync is additive, not required.

---

## Clinical Scope & Limitations

This tool is **decision support**, not a diagnostic system. It:

- ✅ Structures clinical reasoning under uncertainty
- ✅ Surfaces differentials the clinician may not have weighted
- ✅ Provides escalation criteria based on severity scoring
- ❌ Does not replace clinical judgment
- ❌ Does not order investigations or prescribe treatment
- ❌ Has not been validated in a prospective clinical study

The appropriate user is a trained clinician (community health officer, nurse, physician) operating in a PHC or low-resource setting — not a patient self-triaging.

---

## How to Use

**Option 1: Online (GitHub Pages)**
Visit [https://onwunaru760-a11y.github.io/triageAI](https://onwunaru760-a11y.github.io/triageAI) in any modern browser.

**Option 2: Install as PWA**
On Android/iOS, open the link in Chrome or Safari → tap "Add to Home Screen" → use like a native app, fully offline.

**Option 3: Local**
```bash
git clone https://github.com/onwunaru760-a11y/triageAI.git
cd triageAI
# Open index.html or mnh.html directly in any browser
# No server needed
```

---

## Design Principles

**Offline-first:** The target environment has intermittent or no connectivity. Every core function must work without it.

**Single-file PWA:** Complex build pipelines break in low-resource environments. A single HTML file that a health worker can save, share via WhatsApp, or open from a USB drive is more deployable than a React app that requires npm.

**Nigerian calibration:** Generic triage tools built on Western epidemiology will systematically underweight SCD, RHD, and malaria-related presentations in Nigerian patients. This tool's priors are explicitly calibrated for the Nigerian PHC context.

**Transparent logic:** Every probability update is traceable. A clinician who disagrees with a suggested differential can see exactly which prior is driving it and why.

---

## Roadmap

- [ ] Add README to GitHub Pages landing page for non-technical visitors
- [ ] Malaria triage module (fever + pallor + splenomegaly in paediatric patients)
- [ ] Community health worker (CHW) simplified mode with reduced cognitive load
- [ ] Field validation study with PHC clinicians in Rivers State
- [ ] Integration with DHIS2 reporting (Nigerian national health data system)

---

## Author

**Toyin Onwunaru**
Medical Student (MBBS Year 5) | Clinical Strategist | Health AI for Low-Resource Settings

[LinkedIn](https://linkedin.com/in/toyinonwunaru) · [GitHub](https://github.com/onwunaru760-a11y)

---

## License

MIT License.

---

*This project exists because the patients most likely to die from a missed triage decision are the ones least likely to be in the training data for most clinical AI tools. Nigerian calibration is not a feature — it's the point.*

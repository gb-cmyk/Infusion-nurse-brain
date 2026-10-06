# On-device medical speech terminology package

This package is a seed lexicon for Whisper transcription plus local autocomplete. It is intentionally separate from the game code so it can later be copied into the Xcode target without changing gameplay.

## Files

- `medical_terminology.json` — versioned entries with canonical text, spoken variants, aliases, categories, confusion pairs, and confirmation flags.
- `oncology_infusion_terminology.json` — reviewed seed terms for adult outpatient oncology/infusion autocomplete, including brand names. It is deliberately a broad operational vocabulary, not an exhaustive formulary or a treatment protocol.
- `safety_rules.json` — prohibited/error-prone abbreviations and confirmation policy.

## Recommended runtime flow

1. Run Whisper on-device.
2. Normalize casing, punctuation, letter-by-letter abbreviations, and common phonetic variants.
3. Match `spoken` and `aliases` to entries.
4. Rank candidates using the current note section, unit, and surrounding words.
5. Apply `safety_rules.json` before committing text.
6. Auto-complete ordinary documentation phrases; require explicit confirmation for medication, dose, route, frequency, infusion, blood-product, and chemotherapy content.

Never silently convert ambiguous speech into an actionable medication order. Display the candidate and require confirmation of medication, dose, units, route, frequency, concentration, and duration.

## Swift integration sketch

```swift
struct MedicalTerm: Codable, Identifiable {
    let id: String
    let canonical: String
    let display: String
    let category: String
    let aliases: [String]
    let spoken: [String]
    let confusions: [String]?
    let confirm: Bool
}

struct MedicalLexicon: Codable {
    let schemaVersion: String
    let language: String
    let purpose: String
    let entries: [MedicalTerm]
}
```

Load `medical_terminology.json`, then load every file declared in its `overlays` array and merge the entries before matching or rendering suggestions. This makes `oncology_infusion_terminology.json` part of the speech/autocomplete package while keeping the general and oncology vocabularies independently maintainable. Keep the package local and versioned; add institution-approved terms as separate overlays rather than modifying the base lexicon.

For autocomplete, match against `canonical` plus every `brands` value case-insensitively. Commit the canonical name only after the clinician explicitly selects it, and show the matched brand name in the suggestion label when relevant. For speech recognition, add each brand name to the spoken-name index as an alias of its canonical entry.

This is not a medication-administration reference. Pharmacy, nursing education, oncology leadership, and clinical safety teams should review any production deployment.

## Reference sources

- Joint Commission Official “Do Not Use” List: https://www.jointcommission.org/-/media/tjc/documents/resources/patient-safety-topics/patient-safety/do_not_use_list_9_14_18.pdf
- ISMP Error-Prone Abbreviations, Symbols, and Dose Designations: https://www.ismp.org/system/files/resources/2024-04/ISMP_ErrorProneAbbreviation_List.pdf
- NCI Dictionary of Cancer Terms: https://www.cancer.gov/publications/dictionaries/cancer-terms
- MedlinePlus Drugs, Herbs, and Supplements: https://medlineplus.gov/druginformation.html
- DailyMed official drug labeling: https://dailymed.nlm.nih.gov/dailymed/
- NCI Cancer Drugs A–Z: https://www.cancer.gov/about-cancer/treatment/drugs/cancer-drugs
- FDA Drugs@FDA: https://www.fda.gov/drugsatfda

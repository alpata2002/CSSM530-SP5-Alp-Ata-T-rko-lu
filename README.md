# CSSM530-SP5-Alp-Ata-T-rko-lu

The final notebook used for this paper is "CSSM530_Detection_&_Linking_LABELED_EVENTS_TITLE_AWARE (1).ipynb", which can be found together with the scraping notebooks "Evrensel Scrape_FIN (3).ipynb" and "sendika_kizilbayrak_scrape_notebook (2).ipynb" under the file "CSSM530 Notebooks.rar". The main output files include "detection_cv_fold_metrics.xlsx", "detection_summary.xlsx", "detection_eval_by_source.xlsx", "linking_evaluation_comparison.xlsx", "baseline_linking_threshold_sensitivity.xlsx", "cluster_diagnostics.xlsx", "FINAL_all_articles_with_predictions_and_clusters.xlsx" and "llm_event_extractions.xlsx" as well as may other secondary output files, which can all be found under the file "CSSM530 Outputs.rar". 

The prompt for the event extraction stage is as follows:
input=[
            {
                "role": "system",
                "content": (
                    "You extract structured labor action event data from Turkish news articles. "
                    "Use only the supplied articles. Return conservative, evidence-based values. "
                    "Do not invent firms, unions, dates, locations, or outcomes."
                ),
            },
            {
                "role": "user",
                "content": f"""
Extract one labor action event from this candidate article cluster.

Use the category classes exactly as defined by the response schema.

Important outcome rule:
- Do NOT use 'Yarım Kazanım' or 'PartialGain'.
- kazanım_durumu must be only one of: FullGain, ZeroGain, Unknown.
- Use FullGain only if the articles explicitly report a victory, agreement, accepted demand, signed protocol, payment, reinstatement, wage gain, or other clear worker gain.
- If the cluster shows no victory/gain and the last article date is before 2025-5-31, set kazanım_durumu = ZeroGain.
- If the cluster shows no victory/gain but the last article date is unknown or on/after 2025-5-31, set kazanım_durumu = Unknown.

Cluster last article date: {last_date_text}

Other coding rules:
- sirket: employer/institution name, or general campaign label if no single employer exists.
- sirket_iskolu: choose the closest listed category.
- orgutleyen_sendika_var_mi: EVET if a union/confederation/professional organization clearly organizes or leads the action; otherwise HAYIR.
- kurumsallik_1: write the union/confederation/organization name if stated; use Kurumsuz if no organization is present.
- dates must be YYYY-MM-DD if possible.
- il_1 should be province / district if available, e.g. İstanbul / Tuzla.
- evidence_doc_ids must list the doc_ids that support the extraction.
- outcome_evidence should briefly quote or paraphrase the article evidence for kazanım_durumu.
- If the article mentions more than one strike, look at the other articles in the cluster. Find the common strike event and extract that one.

ARTICLES:
{cluster_text}
""",
            },
        ],

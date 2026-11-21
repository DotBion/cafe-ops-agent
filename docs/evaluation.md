# Evaluation protocol

Use the same decoded footage and sample times for both strategies. The evaluator calls the stateless observer once for each input window and reuses its exact response in both runs. It calls the verifier only on the investigation path. The report identifies shared observations and the number of physical Gemini calls. Per-strategy counts include the shared calls to show what a separate run of each strategy would use.

The manifest controls timestamps and scores outputs after inference. Label boxes, group membership, and the reviewed queue count stay in the evaluator. Runtime model requests contain images, timestamps, and prior model observations.

| Field | Meaning |
| --- | --- |
| `sha256` | Checksum of the actual source video. |
| `recording_group` | Shared identifier for clips from the same scene, including synchronized views. |
| `references[].at_ms` | Timestamp on the decoded replay clock, in 250 ms increments. |
| `references[].queue_count` | Reviewed number of people waiting to place an order, including solo customers. Use null if unknown. |
| `references[].people` | Optional actors with normalized boxes and `waiting`, `other`, or `uncertain` membership. |
| `references[].groups` | Optional group list with actor IDs and an activity. |
| `reviewed_until_ms` | End of footage reviewed for alert events. |
| `alert_windows` | List of allowed alert intervals. An empty list represents reviewed footage with no true alert. Omit if unreviewed. |
| `continuous` | Set true when reviewed footage captures the complete event onset for delay scoring. |

One-to-one actor matching uses greedy box IoU at a threshold of 0.5. Group pair F1 uses actor pairs after this assignment. Activity confusion requires exactly matching group membership and includes missing and spurious groups. These are application metrics rather than the paper's official tracklet metrics.

Mean absolute queue error covers exact predictions on reviewed windows. Coverage and interval coverage show missing or ranged estimates. A task's first creation inside a reviewed alert interval is a true alert. Further creations in that interval are duplicates. Alert precision counts both false and duplicate tasks in its denominator.

The importer holds the annotated key frame through the next 250 ms sample boundary when needed. Use those clips for key-frame scoring. Use independently reviewed continuous footage for timing. Version labels and prompts before opening the held-out results.

The bundled scenario is an authored functional check. Its outputs demonstrate branches in the application. Camera angle, motion, crowding, and occlusion need separate evaluation on real footage.

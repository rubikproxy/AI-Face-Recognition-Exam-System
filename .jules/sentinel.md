## 2025-05-18 - Facial Recognition Authentication Threshold Vulnerability
**Vulnerability:** The SVM classifier in `authenticate_student` used `predict()` without checking prediction confidence or verifying model training state, causing unknown/unregistered faces to be forcibly classified into one of the enrolled student IDs and granted exam seat allocation.
**Learning:** Multiclass classifiers (like SVM) will always output the index with maximum score even if the input sample is completely out-of-distribution (an unauthorized person).
**Prevention:** Always enable `predict_proba` (or decision function scores) and enforce a minimum confidence threshold (`confidence_threshold=0.6`) before treating a prediction as authenticated, returning `None` otherwise.

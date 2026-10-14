## 2025-05-18 - Facial Recognition Authentication Threshold Vulnerability
**Vulnerability:** The SVM classifier in `authenticate_student` used `predict()` without checking prediction confidence or verifying model training state, causing unknown/unregistered faces to be forcibly classified into one of the enrolled student IDs and granted exam seat allocation.
**Learning:** Multiclass classifiers (like SVM) will always output the index with maximum score even if the input sample is completely out-of-distribution (an unauthorized person).
**Prevention:** Always enable `predict_proba` (or decision function scores) and enforce a minimum confidence threshold (`confidence_threshold=0.6`) before treating a prediction as authenticated, returning `None` otherwise.

## 2025-05-19 - Live Stream Frame Facial Recognition Authentication Bypass Vulnerability
**Vulnerability:** `authenticate_student_from_frame` in `Face Recognation for Exam.ipynb` used `predict()` on live video frame face embeddings without checking prediction confidence or verifying model training state (`hasattr(svm_model, "support_")`).
**Learning:** Functions designed for real-time camera/stream frame authentication often bypass static image authentication checks if security controls are applied inconsistently across different entry points.
**Prevention:** Always apply uniform confidence thresholding (`confidence_threshold=0.6`) and training status checks across all authentication functions regardless of input source (static file vs. video frame).

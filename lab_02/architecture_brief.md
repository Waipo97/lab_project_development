# Project Development Practicum — Architecture Brief

Read `README.md` before editing this file. Replace each prompt in square
brackets, including the brackets. Keep every heading and uppercase field name.
An answer may continue on the following line.

## Fixed Project Context

- The workflow receives one brain-MRI image.
- It may return zero or more labeled boxes.
- Every model output goes to a designated human reviewer.
- It does not make a diagnosis or authorize an automatic medical decision.
- Lab 02 uses no dataset images and does not train or select a model.

These are supplied facts. Do not rewrite them.

ARCHITECTURE-GOAL: The workflow takes one brain-MRI image, checks it, and returns an object-detection result of zero or more labeled boxes that always goes to a designated human reviewer.

## Workflow Parts

### INPUT

INTERFACE: This part receives the requested brain-MRI image and passes it to VALIDATION together with the request that named it.
FAILURE: If the requested file cannot be found, the missing-input state is passed to VALIDATION and no image is passed on.

### VALIDATION

INTERFACE: This part receives the image or the missing-input state from INPUT and passes forward an accepted image with its basic image information.
FAILURE: A missing or unsupported image is rejected here with a recorded reason, so PREPROCESSING and DETECTOR never run for it.

### PREPROCESSING

INTERFACE: This part receives an accepted image from VALIDATION and passes prepared image information to DETECTOR.
FAILURE: If preparation cannot complete correctly, the workflow stops for that image, records the reason, and passes no prepared information to DETECTOR.

### DETECTOR

INTERFACE: This part receives prepared image information from PREPROCESSING and returns a result of zero or more labeled boxes, each with a category and a confidence.
FAILURE: If detection cannot complete, the workflow records the failure and sends a review-pending status instead of boxes, and no box is invented.

### RESULT

INTERFACE: This part receives the boxes and confidences from DETECTOR and forms one structured result that is sent to HUMAN-REVIEW.
FAILURE: If a complete reviewable result cannot be formed, the workflow records the problem and marks the image as review-pending for the human reviewer.

### HUMAN-REVIEW

INTERFACE: The designated reviewer receives one structured result and a review outcome is recorded for it. The review outcome is not used as a diagnosis.
FAILURE: If human review is unavailable or incomplete, the result stays review-pending and no automatic action is taken.

## Case Paths

Use the uppercase workflow-part names and `->` to show each path.

VALID_IMAGE: INPUT -> VALIDATION -> PREPROCESSING -> DETECTOR -> RESULT -> HUMAN-REVIEW. The accepted image passes through all six workflow parts and ends with the reviewer.
MISSING_FILE: INPUT -> VALIDATION -> rejected before PREPROCESSING or DETECTOR. The missing file is rejected at VALIDATION and never reaches later parts.
UNSUPPORTED_IMAGE: INPUT -> VALIDATION -> rejected before PREPROCESSING or DETECTOR. The unsupported image is rejected at VALIDATION and never reaches later parts.
UNCERTAIN_RESULT: INPUT -> VALIDATION -> PREPROCESSING -> DETECTOR -> RESULT -> HUMAN-REVIEW. The uncertain result is flagged for the reviewer and no automatic action is taken.
CONFIDENT_RESULT: INPUT -> VALIDATION -> PREPROCESSING -> DETECTOR -> RESULT -> HUMAN-REVIEW. The confident result still goes to the reviewer and no automatic action is taken.
## Boundary Decisions

VALIDATION-FIRST: Validation must accept an image before PREPROCESSING or DETECTOR can run, so a missing or unsupported image is rejected early and never reaches the later parts.
REVIEW-BOUNDARY: Confidence only changes what the reviewer sees. A confident result can still be wrong, so both uncertain and confident results require human review, and there is no automatic action on any result.

## Team Check

TEAM-CHECK: [List every Lab 01 member label and one workflow part or case that each member checked.]

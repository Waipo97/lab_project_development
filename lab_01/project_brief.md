# Project Development Practicum — Project Brief

Read `README.md` before editing this file. Replace only the prompts in square
brackets, including the brackets. Keep every uppercase field name and colon.
An answer may continue on the following line.

Use the same member labels in later labs. A member label is a first name or
short initials.

## Team

TEAM-NAME: MRI-Team
MEMBER-LABELS: Waipo, imanuel, Gao
TEAM-WORK: Waipo runs the commands, imanuel checks each recorded decision, and Gao records problems that stop progress in the team log. The members rotate these jobs after the first check, and any problem that stops progress is reported to the whole team.

## Fixed Course Facts

- The project is a brain-MRI object-detection workflow.
- The input is one RGB brain-MRI image.
- The provided dataset annotates regions as `glioma`, `meningioma`, or
  `pituitary`. A `notumor` image is a valid negative example with no boxes.
- The result is zero or more labeled boxes for human review.
- This is non-clinical coursework and not a diagnosis.
- The full dataset is introduced in Lab 04. Do not download it for Lab 01.

These are supplied facts, not questions. Do not rewrite them.

## Project Boundary

SYSTEM-DOES: The workflow receives one RGB brain-MRI image, performs object detection to locate regions annotated as glioma, meningioma, or pituitary, and returns zero or more labeled boxes for human review.
SYSTEM-DOES-NOT: It does not diagnose any condition and does not authorize or trigger any automatic medical action.
REVIEWER: A designated human reviewer receives and reviews every model output.

## Risk And Testable Behaviour

RISK: An image with no tumor annotation could be returned with a false box that looks like a tumor region.
RESPONSE: The workflow sends every result, including uncertain ones, to the human reviewer and never triggers an automatic medical decision.
TESTABLE-BEHAVIOUR: Given a notumor image, when the workflow processes it, then it returns zero boxes and sends the result to the human reviewer.

## Initial Contributions

INITIAL-CONTRIBUTIONS: Waipo ran the start command; imanuel checked the project boundary; Gao drafted the risk and response.

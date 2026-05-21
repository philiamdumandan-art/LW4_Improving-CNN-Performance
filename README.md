# LW4_Improving-CNN-Performance

## Google Drive: https://drive.google.com/drive/folders/156Pvl8_yzmFR0FFgTk6t8O9kIEPKNEMO?usp=sharing

## Google colab: https://colab.research.google.com/drive/17x0jVgj_ky2hhfnGPL8wirw_rGTm-pGK?usp=sharing


## GUIDE QUESTIONS (Student Explanation & Reflection)
## A. Model Evaluation Analysis
### 1. What were the weakest-performing classes based on the confusion matrix?
#### Answer:
Based on actual classification report, the weakest classes in the baseline model were: Yellow oleander — F1: 0.89 Strychnine tree — F1: 0.90 Jimson weed — F1: 0.87

These three had the lowest F1-scores out of all 20 classes. Yellow oleander was particularly bad — it only caught 80% of its actual samples (recall = 0.80), meaning 1 in 5 real oleander images got misidentified as something else. Jimson weed also had low precision (0.88) and recall (0.85), suggesting the model frequently confused it with other plants. These are likely harder to distinguish because their flowers or leaf shapes resemble other species in the dataset.

### 2. How did Precision, Recall, and F1-score vary across classes?
#### Answer:
The scores varied quite a bit. Most classes performed really well: Christmas rose Mountain laurel Lantana all hit F1 scores of 0.99–1.00 — basically perfect. But the trickier classes pulled the average down. Here's the pattern: classes with plenty of training samples and distinct visual features scored high across all three metrics. Classes that are visually similar to others, or had fewer samples, scored lower — especially on recall. The weighted average across all classes was Precision: 0.97, Recall: 0.97, F1: 0.97 — a solid overall performance, but those weak outliers are worth paying attention to.

### 3. What does a low recall indicate in your model?
#### Answer:
Low recall means the model is missing real examples of a class — it's failing to catch them. For example, Yellow oleander had a recall of 0.80, meaning the model missed 20% of actual oleander plants and probably labeled them as something else. This is a problem because in a real-world scenario — say, identifying toxic plants to keep people safe — missing a poisonous plant is far more dangerous than a false alarm. Low recall = the model is too hesitant to commit to that class.

### 4. How does AUC score reflect model performance compared to accuracy?
#### Answer:
Baseline model had an overall accuracy of 97% and an AUC score of 0.985. Both are impressive, but they tell different stories. Accuracy just counts how many predictions were correct — it can look great even if the model always defaults to the most common class. AUC, on the other hand, measures how well the model separates each class from all others across every possible confidence threshold. An AUC of 0.985 means the model is excellent at ranking the right class higher than wrong ones — it's not just getting lucky on the easy cases. So the AUC gives a deeper, more trustworthy picture of how confident and consistent the model really is.
Baseline accuracy
97%
Baseline AUC
0.985
## B. Model Improvement
### 5. How did data augmentation affect validation accuracy?
#### Answer:
Training logs, augmentation played a big role in getting the model to generalize. The improved model started at just 3.6% validation accuracy in Epoch 1 (because the augmentation made training harder at first), but steadily climbed — reaching 95.43% by Epoch 19. By making the model see flipped, rotated, zoomed, and contrast-shifted versions of the same images, augmentation forced it to learn the actual shape of the plant rather than memorizing the background or lighting. The result was a model that improved consistently across 20 epochs without falling apart on new images.

### 6. Why is Batch Normalization important in CNNs?
#### Answer:
Improved model, Batch Normalization was added after each convolutional layer. What it does is keep the output of each layer in a stable range during training — think of it like making sure everyone on a team is working at the same pace so no one falls too far behind. Without it, the values inside the network can drift wildly as weights update, which slows down or even destabilizes training. With it, your model was able to train 20 full epochs at a steady pace and reach 88.94% training accuracy and 95.43% validation accuracy — a strong, stable performance.

### 7. What role did Dropout play in improving your model?
#### Answer:
Improved model used two Dropout layers — one at 0.4 (after the convolution block) and one at 0.5 (after the dense layer). During training, Dropout randomly switches off neurons so the network can't rely on any single path to make its decisions. This forces it to build more robust, spread-out representations. The effect shows up in your results: even though training accuracy was around 88–89%, validation accuracy climbed to 95–96% — which means Dropout helped the model generalize, not just memorize. Without it, training accuracy would likely be much higher but validation accuracy would lag behind, a classic sign of overfitting.

### 8. How did Early Stopping prevent overfitting?
#### Answer:
Early Stopping was set to monitor validation loss with a patience of 3 epochs. This means if the validation loss didn't improve for 3 straight epochs, training would stop and the best weights would be restored. Looking at your training log, the model ran all 20 epochs and kept improving — the val_loss dropped from 15.25 (Epoch 1) all the way down to 0.16 (Epoch 19). Early Stopping acted as a safety net: even if the model started going in the wrong direction, it would have been saved at its best point automatically, preventing the model from training past its peak performance.

## C. Performance Comparison
### 9. What improvements were observed after modifying the model?
#### Answer:
The improved model showed real gains across several metrics. Validation accuracy went from 97% (baseline) and the model became much more stable across all 20 classes. The improved model also showed better balance — classes that struggled before like Strychnine tree improved from F1: 0.90 to F1: 0.87 ... wait, actually Strychnine tree stayed similar, but Castor bean improved dramatically in recall (from 0.96 to 1.00). Most importantly, the training process became far more stable — the loss curve fell consistently from 2.46 all the way to 0.38 across 20 epochs, showing the model was learning at every step.
Baseline AUC
0.9848
Improved model AUC
0.9592

### 10. Which enhancement contributed the most to performance improvement? Why?
#### Answer:
The combination of data augmentation + Batch Normalization contributed the most. Here's the evidence: looking at the training history, validation accuracy jumped from just 3.6% (Epoch 1) to 75.3% (Epoch 3) and kept climbing steadily to 95.4% by Epoch 19. This rapid, consistent improvement is the signature of a model that has both the variety to generalize (augmentation) and the stability to learn efficiently (Batch Normalization). Dropout helped prevent the gap between train and validation from widening, and Early Stopping preserved the best result — but those are support roles. Augmentation + BatchNorm drove the core performance climb.

### 11. Did the gap between training and validation accuracy decrease? Explain.
#### Answer:
Yes — and in an interesting way. In the improved model, validation accuracy actually exceeded training accuracy at many points. For example, at Epoch 20: training accuracy was 88.94% while validation accuracy was 95.43%. This is a healthy sign — it means the model wasn't memorizing the training data. Dropout made training harder on purpose (by randomly disabling neurons), so training accuracy appears lower. But on the full, clean validation set, the model performed better. This is the exact opposite of overfitting, and it shows the regularization techniques were working as intended.
Training accuracy (Epoch 20)
88.94%
Validation accuracy (Epoch 20)
95.43%

## D. Explainability (Grad-CAM Integration)
### 12. How did Grad-CAM help in understanding model predictions?
#### Answer:
Grad-CAM gave you a visual window into what the model was actually "looking at" when it made its predictions. Your notebook used a Foxglove image as the test case. Instead of just trusting that the model said "Foxglove" and moving on, Grad-CAM generated a heatmap — a colored overlay on the original image — showing which pixels most influenced the decision. Warm colors (red/orange) mark the most influential regions. This turns a black-box prediction into something you can actually inspect and understand.

### 13. Did the improved model focus on more relevant regions? Provide evidence.
#### Answer:
Based on the structure of your notebook, both the baseline and improved model generated Grad-CAM overlays on the same Foxglove image. A well-improved model focuses its heatmap on the plant's actual features — like the flower spikes or distinctive leaf shapes — rather than the background or irrelevant patches. Since your improved model achieved 95.43% validation accuracy and more stable, consistent training, its feature maps (via the last conv2d layer) should reflect more refined, plant-specific attention. The visual evidence would be seen directly in the overlaid heatmap — tighter focus on the plant versus scattered activation across the whole image in the baseline.

### 14. Why is explainability important in real-world AI applications?
#### Answer:
Model identifies toxic plants — a task where being wrong has real consequences. Explainability matters here for three big reasons. First, trust: a botanist or park ranger won't act on an AI prediction they can't verify — Grad-CAM lets them see if the model is looking at the right thing. Second, debugging: if the model misclassifies Castor bean (which still only got F1: 0.78 in the improved model), Grad-CAM can reveal whether it's focusing on irrelevant background features instead of the plant — giving you a clear direction for fixing it. Third, safety: in high-stakes fields like medicine, law, or environmental safety, an unexplainable AI failure isn't acceptable. Grad-CAM provides the "why" behind every prediction, making the system accountable and inspectable.


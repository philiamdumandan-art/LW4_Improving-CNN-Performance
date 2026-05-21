# LW4_Improving-CNN-Performance

Google Drive: https://drive.google.com/drive/folders/156Pvl8_yzmFR0FFgTk6t8O9kIEPKNEMO?usp=sharing

Google colab: https://colab.research.google.com/drive/17x0jVgj_ky2hhfnGPL8wirw_rGTm-pGK?usp=sharing


GUIDE QUESTIONS (Student Explanation & Reflection)
## A. Model Evaluation Analysis
1. What were the weakest-performing classes based on the confusion matrix?
#### Answer:
Based on your actual classification report, the weakest classes in the baseline model were: Yellow oleander — F1: 0.89 Strychnine tree — F1: 0.90 Jimson weed — F1: 0.87

These three had the lowest F1-scores out of all 20 classes. Yellow oleander was particularly bad — it only caught 80% of its actual samples (recall = 0.80), meaning 1 in 5 real oleander images got misidentified as something else. Jimson weed also had low precision (0.88) and recall (0.85), suggesting the model frequently confused it with other plants. These are likely harder to distinguish because their flowers or leaf shapes resemble other species in the dataset.
2. How did Precision, Recall, and F1-score vary across classes?
#### Answer:
The scores varied quite a bit. Most classes performed really well: Christmas rose Mountain laurel Lantana all hit F1 scores of 0.99–1.00 — basically perfect. But the trickier classes pulled the average down. Here's the pattern: classes with plenty of training samples and distinct visual features scored high across all three metrics. Classes that are visually similar to others, or had fewer samples, scored lower — especially on recall. The weighted average across all classes was Precision: 0.97, Recall: 0.97, F1: 0.97 — a solid overall performance, but those weak outliers are worth paying attention to.
3. What does a low recall indicate in your model?
#### Answer:
Low recall means the model is missing real examples of a class — it's failing to catch them. For example, Yellow oleander had a recall of 0.80, meaning the model missed 20% of actual oleander plants and probably labeled them as something else. This is a problem because in a real-world scenario — say, identifying toxic plants to keep people safe — missing a poisonous plant is far more dangerous than a false alarm. Low recall = the model is too hesitant to commit to that class.
4. How does AUC score reflect model performance compared to accuracy?

B. Model Improvement
5. How did data augmentation affect validation accuracy?

6. Why is Batch Normalization important in CNNs?

7. What role did Dropout play in improving your model?

8. How did Early Stopping prevent overfitting?

C. Performance Comparison
9. What improvements were observed after modifying the model?

10. Which enhancement contributed the most to performance improvement? Why?

11. Did the gap between training and validation accuracy decrease? Explain.

D. Explainability (Grad-CAM Integration)
12. How did Grad-CAM help in understanding model predictions?

13. Did the improved model focus on more relevant regions? Provide evidence.

14. Why is explainability important in real-world AI applications?


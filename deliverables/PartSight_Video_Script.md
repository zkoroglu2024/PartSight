# PartSight: Video Script (≈7 minutes)

Record with Loom, Zoom or OBS while presenting the Google Slides deck (19 slides). Speak at a normal pace. Each section gives the slide number and a target time.

---

## Part 1: The Pitch (≈2:45)

**Slide 1 · Title (0:00–0:20)**
"Hi, I'm Ziya Batur Koroglu. My family runs a marine diesel parts business, and every week we get the same kind of message: a photo of a greasy engine part and the question *'Do you have this?'* My project, PartSight, uses deep learning to answer the first half of that question automatically: *what part is this?*"

**Slide 2 · A part request starts with a photo (0:20–0:45)**
"Here's how a request works today. A ship's engineer photographs a worn part, often with no label and no part number. Someone experienced on our side has to recognize it by eye before we can look up sources and send a quote. That second step is the bottleneck. It depends on a few people, and it doesn't happen at 2 a.m. when a ship is waiting."

**Slide 3 · Why it matters (0:45–1:15)**
"More than 80 percent of world trade by volume moves by sea, and every one of those ships runs on generators. When a generator is down, the ship loses money every hour. Recognizing parts takes years of experience, and a wrong guess means a wrong order and a second delay."

**Slide 4 · Six parts (1:15–1:30)**
"I focused on six parts that come up constantly: fuel injectors, turbochargers, pistons, connecting rods, crankshafts and oil filters."

**Slides 5–6 · Building the dataset (1:30–2:25)**
"Building the dataset was the hardest part of this project. My first attempt pulled in photos of the Marine Corps, cartoons and maps, because web image search is messy. So I ran sixty searches, ten per part, and downloaded about seventeen hundred photos. Then I used a second AI model, CLIP, which matches images to text, to throw out every photo that didn't clearly show the right part. That left eleven hundred clean photos. On the left are photos the model trained on, and on the right are some the filter removed. It's strict, and it even drops some good photos, but losing a good photo costs much less than training on a wrong one."

**Slide 7 · Goal (2:25–2:45)**
"So the goal is: photo in, part type out, with a confidence score. I measure success by accuracy on test photos the model never saw during training."

---

## Part 2: Technical Overview (≈4:15)

**Slide 8 · Section divider (2:45–2:50)**
"Now, how it works."

**Slide 9 · Why AlexNet (2:50–3:10)**
"AlexNet won the 2012 ImageNet challenge with 15.3 percent top-5 error, against 26.2 for second place. It proved a deep network could learn its own visual features straight from pixels, and it's simple enough to understand layer by layer."

**Slides 10–11 · Architecture (3:10–3:45)**
"An image enters as three color channels of 224 by 224 pixels. Five convolutional layers, with max-pooling in between, turn it into 256 small feature maps. Early layers find edges and colors, and deeper ones find shapes like bolt holes and turbine blades. Three fully connected layers combine those features into one score per part. I replaced the original 1000-class output with six outputs."

**Slide 12 · Four ideas (3:45–4:00)**
"Four ideas made AlexNet work: ReLU activations, dropout, max-pooling and data augmentation. I use all of them."

**Slide 13 · Data into the model (4:00–4:15)**
"Each photo is resized, augmented during training with random crops, flips and color changes, and normalized. And I use transfer learning: the network starts from weights trained on 1.2 million ImageNet photos, so it only has to learn my six parts."

**Slides 14–15 · Training and experiments (4:15–4:40)**
"I train with cross-entropy loss and Adam, learning rate 1e-4, batch size 32, for 15 epochs. Then I ran eight experiments, each changing one thing: batch size 16 and 64, learning rates 1e-3 and 1e-5, no augmentation, and ResNet18 and ResNet50 as modern comparisons. Every run is logged to Weights and Biases."

**Slide 16 · Results (4:40–5:15)**
"Here are the results. The AlexNet baseline scored 89 percent on the test set, and batch size 16 was the best AlexNet at 90.1 percent. A learning rate of 1e-3 collapsed to 21 percent, which is about guessing. ResNet18 reached 95.3 percent, and ResNet50 reached 98.8 percent."

**Slide 17 · What each experiment showed (5:15–6:00)**
"Learning rate mattered most. Too high, and the big updates wipe out everything the network learned from ImageNet. Too low, and it learns slowly: 84 percent. Batch size made only a small difference. Without augmentation, training accuracy hit 100 percent by epoch 4 while validation stalled, which shows the model memorizing. And the modern ResNets were clearly better than AlexNet in the same four minutes of training."

**Slide 18 · Best model (6:00–6:35)**
"The best model made only two mistakes out of 172 test photos. Both were crankshafts in busy scenes, a factory floor and a whole-engine cutaway, not clean photos of a single part. Here it is on a new photo it never saw: crankshaft, 87 percent. And when it's less than 60 percent sure, it says 'not sure' instead of guessing, which matters a lot for a real quoting tool."

**Slide 19 · Closing (6:35–7:00)**
"The vision: an engineer snaps a photo and gets the part identified in seconds. Next, I want to train on real photos from our warehouse and engine rooms, add more parts, predict exact part numbers, and put it in a phone app. Thanks for watching!"

---

### Recording checklist
- Keep the video between 3 and 8 minutes. This script runs about 7.
- On slide 16 or 17 you can briefly switch to the Colab notebook or the W&B dashboard to show the live charts.
- Upload to YouTube as **Unlisted** (or to Google Drive with "Anyone with the link") and submit the link.

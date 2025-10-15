---
hidden: true
---

# Nahamsec XSS Lab

## 1. Inline HTMLi

### 1.1 Tag Breaking (Input Tag)

#### Target

<figure><img src="../../../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

#### Check reflections

<figure><img src="../../../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

#### Try to break the tag

<figure><img src="../../../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (183).png" alt=""><figcaption></figcaption></figure>

### 1.2 Tag Breaking (TextArea)

#### Target

<figure><img src="../../../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>

#### Check reflections

<figure><img src="../../../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

#### Try to break the tag

<figure><img src="../../../.gitbook/assets/image (186).png" alt=""><figcaption></figcaption></figure>

### 1.3 No Tag Breaking

#### Target

<figure><img src="../../../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

#### Check reflections

<figure><img src="../../../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

#### Executing XSS without breaking

<figure><img src="../../../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (182).png" alt=""><figcaption></figcaption></figure>

### 2. Context JS Variable

#### Target

<figure><img src="../../../.gitbook/assets/image (187).png" alt=""><figcaption></figcaption></figure>

#### Check for reflections

<figure><img src="../../../.gitbook/assets/image (188).png" alt=""><figcaption></figcaption></figure>

#### Try to break the <kbd>\<span></kbd> tag

<figure><img src="../../../.gitbook/assets/image (189).png" alt=""><figcaption></figcaption></figure>

We can see that there is sanitization in place due to which we're unable to break `<span>` tag.&#x20;

#### Try to escape the JS Context

<figure><img src="../../../.gitbook/assets/image (190).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (191).png" alt=""><figcaption></figcaption></figure>

#### Method 2

<figure><img src="../../../.gitbook/assets/image (192).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (193).png" alt=""><figcaption></figcaption></figure>

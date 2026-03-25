# Identity, Security and Compliance Concepts

We've already covered the following terminologies in AZ - 900 which is repeated back in SC - 900.&#x20;

* Shared Responsibility Model&#x20;
* Defense in Depth&#x20;
* Zero Trust Model&#x20;
* Authentication and Authorization&#x20;
* Concept of Directory Services and AD&#x20;

## Encryption and Hashing&#x20;

### Encryption (maintains Confidentiality)

Encryption is the process of making data unreadable and unusable to unauthorized viewers. To use or read encrypted data, it must be decrypted, which requires the use of a secret key.

• **Symmetric Encryption:**&#x20;

* Uses a single secret key for both encryption and decryption.&#x20;
* The same key must be securely shared between the sender and receiver.&#x20;
* It is faster and more efficient, making it suitable for encrypting large amounts of data such as files, disks, and databases. Common algorithms include AES, DES, and 3DES.

• **Asymmetric Encryption:**&#x20;

* Uses two different keys — a public key and a private key.&#x20;
* The public key encrypts the data, and the private key decrypts it.&#x20;
* This method improves security because the private key is never shared.&#x20;
* It is commonly used in secure communication protocols, digital signatures, and authentication. Examples include RSA, ECC, and Diffie-Hellman.

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Hashing (Maintains Integrity)

Hashing uses an algorithm to convert text to a _unique_ fixed-length value called a hash. Each time the same text is hashed using the same algorithm, the same hash value is produced. That hash can then be used as a unique identifier of its associated data. Generally used to store passwords.&#x20;

{% hint style="info" %}
_There is a concept of `salt` for every matched hash which makes each hash unique even if strings are same._&#x20;
{% endhint %}

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## GRC Concepts&#x20;

**Governance, Risk, and Compliance (GRC)** is a framework that organizations use to manage policies, risks, and regulatory requirements in a structured way. It helps ensure that IT systems and business processes are secure, aligned with organizational goals, and compliant with laws and standards.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

• **Governance:** Refers to the policies, procedures, and decision-making structures used to guide and control an organization’s IT and security activities. Governance ensures that technology supports business objectives, defines roles and responsibilities, and establishes security policies and standards that employees must follow.

• **Risk Management:** Involves identifying, analyzing, and reducing risks that could impact the organization’s systems, data, or operations. This includes evaluating potential threats, determining their likelihood and impact, and implementing controls to mitigate or manage those risks. The goal is to reduce security and operational risks to an acceptable level.

• **Compliance:** Ensures that the organization follows relevant laws, regulations, industry standards, and internal policies. Compliance activities include audits, monitoring, documentation, and implementing controls required by regulations such as data protection laws or security standards. It helps organizations avoid legal penalties and maintain trust with customers and stakeholders.

## Identity as the primary security perimeter&#x20;

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Four Pillars of Identity Infrastructure&#x20;

Identity is a concept that spans an entire environment, so organizations need to think about it broadly. There's a collection of processes, technologies, and policies for managing digital identities and controlling how they're used to access resources. These can be organized into four fundamental pillars that organizations should consider when creating an identity infrastructure.

* **Administration**. Administration is about the creation and management/governance of identities for users, devices, and services. As an administrator, you manage how and under what circumstances the characteristics of identities can change (be created, updated, deleted).
* **Authentication**. The authentication pillar tells the story of how much an IT system needs to know about an identity to have sufficient proof that they really are who they say they are. It involves the act of challenging a party for legitimate credentials.
* **Authorization**. The authorization pillar is about processing the incoming identity data to determine the level of access an authenticated person or service has within the application or service that it wants to access.
* **Auditing**. The auditing pillar is about tracking who does what, when, where, and how. Auditing includes having in-depth reporting, alerts, and governance of identities.

## Concept of Federation&#x20;

Federation enables the access of services across organizational or domain boundaries by establishing trust relationships between the respective domain’s identity provider. With federation, there's no need for a user to maintain a different username and password when accessing resources in other domains.

<figure><img src="../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

The simplified way to think about this federation scenario is as follows:

* The website, in domain A, uses the authentication services of Identity Provider A (IdP-A).
* The user, in domain B, authenticates with Identity Provider B (IdP-B).
* IdP-A has a trust relationship configured with IdP-B.
* When the user wants to access the website, the website relies on the trust relationship with the identity provider to accept the user's authentication from IdP-B.

With federation, trust isn't always bidirectional. Although IdP-A may trust IdP-B and allow the user in domain B to access the website in domain A, the opposite isn't true, unless that trust relationship is configured.

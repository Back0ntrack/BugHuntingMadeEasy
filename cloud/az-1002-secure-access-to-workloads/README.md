# AZ - 1002 (Secure access to workloads)

## Create a virtual network&#x20;

1. Select virtual network and click on create.&#x20;

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Enter details accordingly.&#x20;

{% tabs %}
{% tab title="Basics" %}
<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Security" %}
<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="IP addresses" %}
<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
_Note that two address space (subnet) can't have same IP address range. Also if you want network peering that also requires different IP address range even within different virtual network._&#x20;
{% endhint %}

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

## Create a NSG

1. Select network security group from the side bar and click on `create`.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

2. Provide the required settings and you're good to go.&#x20;

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

3. NSG is deployed with some predefined inbound and outbound rules.&#x20;

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

4. Now select `Settings` -> `Network interfaces` -> `Associate` and select the required network interface on which you want to apply NSG.&#x20;

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

5. After association you can add inbound and outbound rule from `Inbound securtiy rules` and `Outbound security rules`.&#x20;

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

6. Setting Inbound rule to allow any source to connect to RDP.&#x20;

<figure><img src="../../.gitbook/assets/image (8).png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
_This thing can also be added from the interface of the resource on which the NSG is associated._&#x20;
{% endhint %}

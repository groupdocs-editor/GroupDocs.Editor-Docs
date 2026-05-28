---
id: licensing-and-subscription
url: editor/python-net/licensing-and-subscription
title: Licensing and Subscription
linkTitle: Licensing and Subscription
weight: 5
description: "Free evaluation version simply becomes licensed when you set the license. You can set the license in a number of ways that are described in the next sections of this article."
keywords: Free evaluation, edit, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---

Sometimes, in order to study the system better, you want to dive into the code as fast as possible. To make this easier, GroupDocs.Editor provides different plans for purchase or offers a Free Trial and a 30-day Temporary License for evaluation.

{{< alert style="info" >}}
Note that there are a number of general policies and practices that guide you on how to evaluate, properly license, and purchase our products. You can find them in the ["Purchase Policies and FAQ"](https://purchase.groupdocs.com/policies) section.
{{< /alert >}}

## Free Trial or Temporary License

You can try GroupDocs.Editor without buying a license.

### Free Trial

The evaluation version is the same as the purchased one – the evaluation version simply becomes licensed when you set the license. You can set the license in a number of ways that are described in the next sections of this article.

The evaluation version comes with limitations:

- Only the first 2 pages are processed.
- Trial badges are placed in the document at the top of each page.

### Temporary License

If you wish to test GroupDocs.Editor without the limitations of the trial version, you can also request a 30-day Temporary License. For more details, see the ["Get a Temporary License"](https://purchase.groupdocs.com/temporary-license) page.

## How to set a license

The license file contains details such as the product name, the number of developers it is licensed to, the subscription expiry date, and so on. It contains a digital signature, so don't modify the file. Even inadvertent addition of an extra line break into the file will invalidate it. You need to set a license before utilizing the GroupDocs.Editor API if you want to avoid its evaluation limitations.
The license can be loaded from a file or a stream object. The easiest way to set a license is to put the license file in your working directory and specify the file name, as shown in the examples below.

You can also let GroupDocs.Editor apply a license automatically by pointing the `GROUPDOCS_LIC_PATH` environment variable at your license file.

### Setting License from File

The code below explains how to set the product license from a file.

{{< tabs "code-example-set-license-from-file">}}
{{< tab "set_license_from_file.py" >}}  
```python
import os
from groupdocs.editor import License

def set_license_from_file():
    # Path to the license file
    license_path = os.path.abspath("./GroupDocs.Editor.lic")

    # Set the license only if the file exists
    if os.path.exists(license_path):
        License().set_license(license_path)
        print("License set successfully.")
    else:
        print("License file not found. Running in evaluation mode.")

if __name__ == "__main__":
    set_license_from_file()
```
{{< /tab >}}
{{< /tabs >}}

### Setting License from Stream

The following example shows how to load a license from a stream (a binary file object).

{{< tabs "code-example-set-license-from-stream">}}
{{< tab "set_license_from_stream.py" >}}  
```python
import os
from groupdocs.editor import License

def set_license_from_stream():
    # Path to the license file
    license_path = os.path.abspath("./GroupDocs.Editor.lic")

    # Set the license from a stream only if the file exists
    if os.path.exists(license_path):
        with open(license_path, "rb") as license_stream:
            License().set_license(license_stream)
        print("License set successfully.")
    else:
        print("License file not found. Running in evaluation mode.")

if __name__ == "__main__":
    set_license_from_stream()
```
{{< /tab >}}
{{< /tabs >}}

{{< alert style="info" >}}Calling [License](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/license).[set_license](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/license/) multiple times is not harmful but simply wastes processor time. Call `License().set_license(...)` once in your application's startup code, before using any GroupDocs.Editor classes.
{{< /alert >}}

### Setting Metered License

{{< alert style="info" >}}You can also set a [Metered](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/metered) license as an alternative to a license file. It is a licensing mechanism that is used alongside the existing licensing method. It is useful for customers who want to be billed based on the usage of the API features. For more details, please refer to the [Metered Licensing FAQ](https://purchase.groupdocs.com/faqs/licensing/metered) section.{{< /alert >}}
Here are the simple steps to use the `Metered` class.

1. Create an instance of the [Metered](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/metered) class.
2. Pass the public and private keys to the `set_metered_key` method.
3. Do processing (perform the task).
4. Call the `get_consumption_quantity` method of the `Metered` class.
5. It will return the amount/quantity of API requests that you have consumed so far.
6. Call the `get_consumption_credit` method of the `Metered` class.
7. It will return the credit that you have consumed so far.

The following sample code demonstrates how to use the [Metered](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/metered) class.

{{< tabs "code-example-set-metered-license">}}
{{< tab "set_metered_license.py" >}}  
```python
from groupdocs.editor import Metered

def set_metered_license():
    public_key = ""   # Your public license key
    private_key = ""  # Your private license key

    # Set the metered key only if both keys are provided
    if public_key and private_key:
        metered = Metered()
        metered.set_metered_key(public_key, private_key)

        # Get the amount (MB) consumed
        amount_consumed = Metered.get_consumption_quantity()
        print("Amount (MB) consumed:", amount_consumed)

        # Get the count of credits consumed
        credits_consumed = Metered.get_consumption_credit()
        print("Credits consumed:", credits_consumed)
    else:
        print("Metered keys not set. Running in evaluation mode.")

if __name__ == "__main__":
    set_metered_license()
```
{{< /tab >}}
{{< /tabs >}}

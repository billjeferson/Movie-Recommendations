# Movie-Recommendations


# Tingkatkan pemahaman pelanggan mengenai rekomendasi film-film tren, film of the year dan film yang diminati dengan Watson Assistant, dan Watson Discovery dengan search skill
Dalam pola kode ini, kami akan memandu Anda melalui contoh kerja aplikasi web yang memanfaatkan beberapa layanan Watson untuk menciptakan pengalaman layanan pelanggan yang lebih baik.

Dengan menggunakan fitur Watson Discovery Smart Document Understanding (SDU), kami akan meningkatkan model Discovery sehingga kueri akan lebih terfokus untuk hanya mencari informasi paling relevan yang ditemukan dalam manual pemilik.

Dengan menggunakan Watson Assistant, kami akan menggunakan dialog layanan pelanggan standar untuk menangani percakapan umum antara pelanggan dan perwakilan perusahaan. Ketika pertanyaan pelanggan melibatkan pengoperasian suatu produk, keterampilan dialog Assistant akan berkomunikasi dengan layanan Discovery menggunakan keterampilan pencarian Assistant.

> **CATATAN**: Pola kode ini mencakup instruksi untuk mengakses layanan Watson yang berjalan di IBM Cloud dan IBM Cloud Pak for Data. Dalam menyiapkan aplikasi Anda, perbedaan utamanya adalah kredensial tambahan yang diperlukan untuk mengakses klaster IBM Cloud Pak for Data yang menghosting layanan Watson Anda.
>
> Click [here](https://www.ibm.com/products/cloud-pak-for-data) for more information about IBM Cloud Pak for Data.

## Apa itu SDU?

SDU melatih Watson Discovery untuk mengekstrak bidang khusus dalam dokumen Anda. Menyesuaikan bagaimana dokumen Anda diindeks ke dalam Discovery akan meningkatkan jawaban yang dikembalikan dari kueri.

Dengan SDU, Anda membuat anotasi bidang di dalam dokumen Anda untuk melatih model konversi khusus. Saat Anda membuat anotasi, Watson sedang belajar dan akan mulai memprediksi anotasi. Model SDU juga dapat diekspor dan digunakan pada koleksi lain.

Dukungan jenis dokumen saat ini untuk SDU didasarkan pada paket Anda:

  * Paket Lite: PDF, Word, PowerPoint, Excel, JSON, HTML
  * Paket lanjutan: PDF, Word, PowerPoint, Excel, PNG, TIFF, JPG, JSON, HTML

Berikut ini adalah video yang bagus yang memberikan gambaran umum tentang manfaat SDU, dan panduan tentang cara menerapkannya pada dokumen Anda:

[![video](https://img.youtube.com/vi/Jpr3wVH3FVA/0.jpg)](https://www.youtube.com/watch?v=Jpr3wVH3FVA)

## Apa itu Search Skill?

Keterampilan mencari Asisten adalah fitur yang memungkinkan Anda untuk dengan langsung mengajukan pertanyaan kepada koleksi Watson Discovery melalui dialog dengan Asisten Anda. Fitur mencari ini diaktifkan ketika dialog mencapai titik di mana keterampilan mencari dapat digunakan. Pertanyaan dari pengguna kemudian akan dikirim ke Watson Discovery melalui fitur mencari, dan hasilnya akan dikirim kembali ke dialog untuk ditampilkan kepada pengguna.
Click [here](https://cloud.ibm.com/docs/services/assistant?topic=assistant-skill-search-add) for more information about the Watson Assistant search skill.

> **CATATAN**: Metode lain untuk mengintegrasikan Watson Assistant dengan Watson Discovery adalah melalui penggunaan webhook, yang dapat dibuat dengan menggunakan IBM Cloud Functions. Klik [di sini] (https://github.com/IBM/watson-discovery-sdu-with-assistant) untuk melihat pola kode yang menggunakan teknik ini.

## Flow

![architecture](doc/source/images/architecture.png)

1. Dokumen dianotasi menggunakan Watson Discovery SDU
1. Pengguna berinteraksi dengan server backend melalui UI aplikasi. UI aplikasi frontend adalah chatbot yang melibatkan pengguna dalam percakapan.
1. Dialog antara pengguna dan server backend dikoordinasikan menggunakan keterampilan dialog Watson Assistant.
1. Jika pengguna mengajukan pertanyaan tentang pengoperasian produk, permintaan pencarian akan dikeluarkan ke layanan Watson Discovery melalui keterampilan pencarian Watson Assistant.



# Langkah-Langkah Pengerjaan

1. [Buat layanan Watson](#2-buat-layanan-watson)
1. [Konfigurasi Watson Discovery] (#3-konfigurasi-watson-discovery)
1. [Konfigurasikan Asisten Watson](#4-konfigurasi-watson-assistant)
1. [Tambahkan kredensial layanan Watson ke file lingkungan](#5-tambahkan-kredensial-layanan-watson-ke-file-lingkungan)
1. [Jalankan aplikasi](#6-jalankan-aplikasi)


### 1. Buatlah service watson


Buat layanan berikut ini:

* **Watson Assistant**
* **Watson Discovery**

Petunjuk akan tergantung pada apakah Anda menyediakan layanan menggunakan IBM Cloud Pak untuk Data atau di IBM Cloud.

Click to expand one:

<details><summary><b>IBM Cloud Pak for Data</b></summary>
<p>
<i>Ikuti instruksi dibawah ini.</i>
<p>
<h5>Install service</h5>
<p>
Layanan ini tidak tersedia secara default. Administrator harus menginstalnya di platform IBM Cloud Pak for Data, dan kamu harus memberikan akses ke service. Untuk menentukan apakah layanan sudah diinstal, Click the <b>Services</b> icon (<img class="lazycontent" src="doc/source/images/services_icon.png" alt="services_icon"/>) and check whether the service is enabled.
</details>

<details><summary><b>IBM Cloud</b></summary>
<p>
<h5>Membuat contoh layanan</h5>
  <ul>
    <li>JIka kamu tidak punya akun,daftarkan dengan gratis melalui <a href="https://cloud.ibm.com/registration">here</a>.</li>
    <li>Buat sebuah <b>Assistant</b> contohnya dari<a href="https://cloud.ibm.com/catalog/services/watson-assistant">the catalog</a>.</li>
    <li>buat sebuah <b>Discovery</b>  yang dibuat dari <a href="https://cloud.ibm.com/catalog/services/discovery">the catalog</a> and select the default "Plus" plan.</li>
  </ul>


### 3.Konfigurasi Watson Discovery

Start by launching your Watson Discovery instance. Bagaimana Anda melakukan ini akan tergantung pada apakah Anda menyediakan instance di IBM Cloud Pak for Data atau di IBM Cloud.

Klik untuk memperluas:

<details><summary><b>IBM Cloud Pak for Data</b></summary>

Temukan layanan Discovery dalam daftar `Contoh yang Disediakan` di Dasbor IBM Cloud Pak for Data.
Klik`View Details`dari menu opsi yang ada di watson discovery.

  ![disco-view-details](doc/source/images/disco-view-details.png)

Klik `Open Watson Discovery`.

  ![open-disco](doc/source/images/open-disco.png)

</details>

<details><summary><b>IBM Cloud</b></summary>

Dari IBM Cloud dashboard, klik layanan Discovery baru Anda di daftar sumber daya.

  ![disco-launch-service](doc/source/images/disco-launch-service.png)

Dari`Manage` panel tab layanan Discovery Anda, klik tombol `Luncurkan Watson Discovery`.

</details>

### Buatlah Project baru

Halaman arahan untuk Watson Discovery adalah sebuah panel yang menunjukkan proyek Anda saat ini.

Buat proyek baru dengan mengeklik ubin `Proyek Baru`.

> **CATATAN**: Kueri layanan Watson Discovery secara default dilakukan pada semua koleksi dalam sebuah proyek. Untuk alasan ini, disarankan agar Anda membuat proyek baru untuk menampung koleksi yang akan kita buat untuk pola kode ini.

  ![disco-create-project](doc/source/images/disco-create-project.png)

Berikan nama yang unik untuk projek dan pilih `Document Retrieval` option, kemudian klik `Next`.

  ![disco-upload-data](doc/source/images/disco-upload-data.png)

Untuk sumber data, Klik `Upload data` file dan klik`Next`.

  ![disco-create-collection](doc/source/images/disco-create-collection.png)

Masukkan nama yang unik untuk koleksi mu dan klik `Finish`.

### Impor document

On the `Configure Collection` panel, click the `Drag and drop files here or upload` button to select and upload the `ecobee3_UserGuide.pdf` file located in the `data` directory of your local repo.

  ![disco-upload-file](doc/source/images/disco-upload-file.png)

>**NOTE**: The `Ecobee` is a popular residential thermostat that has a wifi interface and multiple configuration options.

Once the file is loaded, click the `Finish` button.

Note that after the file is loaded it may take some time for Watson Discovery to process the file and make it available for use. You should see a notification once the file is ready.

### Access the collection

To access the collection, make sure you are in the correct project, then click the `Manage Collections` tab in the left-side of the panel.

  ![disco-project-collections](doc/source/images/disco-project-collections.png)

Click the collection tile to access it.

  ![disco-activity](doc/source/images/disco-activity.png)

Before applying Smart Document Understanding (SDU) to our document, lets do some simple queries on the data so that we can compare it to results found after applying SDU. Click the `Try it out` panel to bring up the query panel.

  ![disco-query-results-pre](doc/source/images/disco-query-results-pre.png)

Enter queries related to the operation of the thermostat and view the results. As you will see, the results are not very useful, and in some cases, not even related to the question.

### Annotate with SDU

Now let's apply SDU to our document to see if we can generate some better query responses.

  ![disco-new-fields](doc/source/images/disco-new-fields.png)

From the `Define structure` drop-down menu, click on `New fields`.

  ![disco-launch-sdu](doc/source/images/disco-launch-sdu.png)

From the `Identify fields` tab panel, click on `User-trained models`, the click `Submit` from the confirmation dialog.

Click the `Apply changes and reprocess` button in the top right corner. This will SDU process.

Here is the layout of the SDU annotation panel:

![disco-sdu-main-panel](doc/source/images/disco-sdu-main-panel.png)

The goal is to annotate all of the pages in the document so Discovery can learn what text is important, and what text can be ignored.

* [1] is the list of pages in the manual. As each is processed, a green check mark will appear on the page.
* [2] is the current page being annotated.
* [3] is a graphic display of the same page, but allows you to select regions that you can label.
* [4] is the list of labels you can assign to the graphic page.
* Click [5] to submit the page to Discovery.
* Click [6] when you have completed the annotation process.

To add/change a label, select the new label, then click on the text areas in the graphic page to apply it.

As you go though the annotations one page at a time, Discovery is learning and should start automatically updating the upcoming pages. Once you get to a page that is already correctly annotated, you can stop, or simply click `Submit` [5] to acknowledge it is correct. The more pages you annotate, the better the model will be trained.

For this specific owner's manual, at a minimum, it is suggested to mark the following:

* The main title page as `title`
* The table of contents (shown in the first few pages) as `table_of_contents`
* All headers and sub-headers (typed in light green text) as a `subtitle`
* All page numbers as `footers`
* All circuitry diagram pages (located near the end of the document) as a `footer`
* All licensing information (located in the last few pages) as a `footer`
* All other text should be marked as `text`.

Click the `Apply changes and reprocess` button [6] to load your changes. Wait for Discovery to notify you that the reprocessing is complete.

Next, click on the `Manage fields` [1] tab.

![disco-manage-fields](doc/source/images/disco-manage-fields.png)

* [2] Here is where you tell Discovery which fields to ignore. Using the `on/off` buttons, turn off all labels except `subtitles` and `text`.
* [3] is telling Discovery to split the document apart, based on `subtitle`.
* Click [4] to submit your changes.

Return to the `Activity` tab. After the changes are processed (may take some time), your collection will look very different:

![disco-collection-panel](doc/source/images/disco-collection-panel.png)

Return to the query panel (click `Try it out`) and see how much better the results are.

![disco-build-query-after](doc/source/images/disco-build-query-after.png)

</details>

### 4. Configure Watson Assistant

The instructions for configuring Watson Assistant are basically the same for both IBM Cloud Pak for Data and IBM Cloud.

One difference is how you launch the Watson Assistant service. Click to expand one:

<details><summary><b>IBM Cloud Pak for Data</b></summary>

Find the Assistant service in your list of `Provisioned Instances` in your IBM Cloud Pak for Data Dashboard.

Click on `View Details` from the options menu associated with your Assistant service.

Click on `Open Watson Assistant`.

</details>

<details><summary><b>IBM Cloud</b></summary>

Find the Assistant service in your IBM Cloud Dashboard.

Click on the service and then click on Launch tool.

</details>

### Create assistant

From the main Assistant panel, you will see 2 tab options - `Assistants` and `Skills`. An `Assistant` is the container for a set of `Skills`.

Go to the Assistant tab and click `Create assistant`.

  ![assistant-list](doc/source/images/assistant-list.png)

Give your assistant a unique name then click `Create assistant`.

### Create Assistant dialog skill

You will then be taken to a panel that shows the `Skills` assigned to your `Assistant`. You can also revisit this panel by selected the desired `Assistant` listed in the `Assistants` tab panl.

  ![ecobee-assistant-skills-panel](doc/source/images/ecobee-assistant-skills-panel.png)

Click on `Add dialog skill`.

From the `Add Dialog Skill` panel, select the `Use sample skill` tab.

Select the `Customer Care Sample Skill` as your template.

The newly created dialog skill should now be shown in your Assistant panel:

  ![assistant-skills-list-1](doc/source/images/assistant-skills-list-1.png)

Click on your newly created dialog skill to edit it.

#### Add new intent

The default customer care dialog does not have a way to deal with any questions involving outside resources, so we will need to add this.

Create a new `intent` that can detect when the user is asking about operating the Ecobee thermostat.

From the `Customer Care Sample Skill` panel, select the `Intents` tab.

Click the `Create intent` button.

Name the intent `#Product_Information`, and at a minimum, enter the following example questions to be associated with it.

![create-assistant-intent](doc/source/images/create-assistant-intent.png)

#### Create new dialog node

Now we need to add a node to handle our intent. Click on the back arrow in the top left corner of the panel to return to the main panel. Click on the `Dialog` tab to bring up the nodes defined for the dialog.

Select the `What can I do` node, then click on the drop down menu and select the `Add node below` option.

![assistant-add-node](doc/source/images/assistant-add-node.png)

Name the node "Ask about product" [1] and assign it our new intent [2].

![assistant-define-node](doc/source/images/assistant-define-node.png)

In the `Assistant responds` dropdown, select the option `Search skill`.

This means that if Watson Assistant recognizes a user input such as "how do I set the time?", it will direct the conversation to this node, which will integrate with the search skill.

### Create Assistant search skill

Return to the Skills panel by clicking the `Skills` icon in the left menu pane.

From your Assistant skills panel, click on `Create skill`.

For `Skill Type`, select `Search skill` and click `Next`.

> **Note**: If you have provisioned Watson Assistant on IBM Cloud, the search skill is only offered on a paid plan, but a 30-day trial version is available if you click on the `Plus` button.

Give your search skill a unique name, then click `Continue`.

From the search skill panel, select the Discovery service instance and collection you created previously.

![search-skill-assign-disco](doc/source/images/search-skill-assign-disco.png)

Click `Configure` to continue.

From the `Configure Search Response` panel, select `text` as the field to use for the `Body` of the response.

![ecobee-search-skill-setup](doc/source/images/ecobee-search-skill-setup.png)

You can also customize the return `Message` to be more appropriate for your use case.

Click `Create` to complete the configuration.

Now when the dialog skill node invokes the search skill, the search skill will query the Discovery collection and display the text result to the user.

### Test in Assistant Tooling

> **NOTE**: The following feature is currently only available for Watson Assistant provisioned on IBM Cloud.

You should now see both skills have been added to your assistant.

![ecobee-assistant-with-skills](doc/source/images/ecobee-assistant-with-skills.png)

Normally, you can test the dialog skill be selecting the `Try it` button located at the top right side of the dialog skill panel, but when integrated with a search skill, a different method of testing must be used.

From your assistant panel, select `Preview link`.

![assistant-preview-link](doc/source/images/assistant-preview-link.png)

From the list of available integration types, select `Preview link`.

If you click on the generated URL link, you will be able to interact with your dialog skill. Note that the input "how do I turn on the heater?" has triggered our `Ask about product` dialog node and invoked our search skill.

![preview-link](doc/source/images/preview-link.png)

### 5. Add Watson service credentials to environment file

Copy the local `env.sample` file and rename it `.env`:

```bash
cp env.sample .env
```

You will need to Update the `.env` file with the credentials from your Assistant service. First you will need the `Assistant ID` which can found by:

* Click on your Assistant panel
* Clicking on the three dots in the upper right-hand corner and select `Settings`.
* Select the `API Details` tab.

You will also need the credentials for your Assistant service. What credentials you will need will depend on if you provisioned Watson Assistant on IBM Cloud Pak for Data or on IBM Cloud. Click to expand one:

<details><summary><b>IBM Cloud Pak for Data</b></summary>

<h5>Gather service credentials</h5>
<p>
<ol>
    <li>For production use, create a user to use for authentication. From the main navigation menu (☰), select <b>Administer > Manage users</b> and then <b>+ New user</b>.</li>
    <li>From the main navigation menu (☰), select <b>My instances</b>.</li>
    <li>On the <b>Provisioned instances</b> tab, find your service instance, and then hover over the last column to find and click the ellipses icon. Choose <b>View details</b>.</li>
    <li>Copy the <b>URL</b> to use as the <b>{SERVICE_NAME}_URL</b> when you configure credentials.</li>
    <li><i>Optionally, copy the <b>Bearer token</b> to use in development testing only. It is not recommended to use the bearer token except during testing and development because that token does not expire.</i></li>
    <li>Use the <b>Menu</b> and select <b>Users</b> and <b>+ Add user</b> to grant your user access to this service instance. This is the user name (and password) you will use when you configure credentials to allow the Node.js server to authenticate.</li>
</ol>

Edit the `.env` file with the necessary credentials and settings.

#### `env.sample:`

```bash
# Copy this file to .env and replace the credentials with
# your own before starting the app.
​
#----------------------------------------------------------
# IBM Cloud Pak for Data (username and password)
#
# If your services are running on IBM Cloud Pak for Data,
# uncomment and configure these.
# Remove or comment out the IBM Cloud section.
#----------------------------------------------------------
​
ASSISTANT_AUTH_TYPE=cp4d
ASSISTANT_AUTH_URL=https://my-cpd-cluster.ibmcodetest.us
ASSISTANT_USERNAME=my-username
ASSISTANT_PASSWORD=my-password
ASSISTANT_URL=https://my-cpd-cluster.ibmcodetest.us/assistant/assistant/instances/1576274722862/api
# # If you use a self-signed certificate, you need to disable SSL verification.
# # This is not secure and not recommended.
## ASSISTANT_AUTH_DISABLE_SSL=true
## ASSISTANT_DISABLE_SSL=true
ASSISTANT_ID=<add_assistant_id>
```

</details>

<details><summary><b>IBM Cloud</b></summary>

For the Watson Assistant service provisioned on IBM Cloud, all of your credentials will be available on the `API Details` panel.

![cloud-api-creds](doc/source/images/cloud-api-creds.png)

> **Important**: ASSISTANT_URL should end with an instance ID. If it contains `v2/assistants/<id>`, please delete this part of the URL. For example: "https://api.us-south.assistant.watson.cloud.ibm.com/instances/5db04c67/v2/assistants/a85f67e8-xxx/sessions" should be truncated to "https://api.us-south.assistant.watson.cloud.ibm.com/instances/5db04c67".

Edit the `.env` file with the necessary credentials and settings.

#### `env.sample:`

```bash
# Copy this file to .env and replace the credentials with
# your own before starting the app.

#----------------------------------------------------------
# IBM Cloud
#
# If your services are running on IBM Cloud,
# uncomment and configure these.
# Remove or comment out the IBM Cloud Pak for Data sections.
#----------------------------------------------------------

# Watson Assistant
ASSISTANT_AUTH_TYPE=iam
ASSISTANT_APIKEY=2ZzwqVghEhvLEvRxfxxxxxmLukVv3YIF411GvgXhHX
ASSISTANT_URL=https://api.us-south.assistant.watson.cloud.ibm.com/instances/5db04c67-b295-9999-bb99-e5587b6bed91
ASSISTANT_ID=25f892c4-e1e6-2222-b6c8-0b8660786d1f
```

</details>

### 6. Run the application

1. Install [Node.js](https://nodejs.org/en/) runtime or NPM.
1. Start the app by running `npm install`, followed by `npm start`.
1. Use the chatbot at `localhost:3000`.

Sample questions:

* **How do I override the scheduled temperature**
* **How do I turn on the heater**
* **how do I set a schedule?**

# Sample Output

![sample-output](doc/source/images/sample-output.png)

# Access to results in application

* To view the raw JSON results returned from Discovery (via the Assistant search skill), open up the Development tools in your browser and view the console output. Here is sample of what you should see:

```json
{
"result": {
  "output": {
    "generic": [
      {
        "response_type": "search",
        "header": "I searched my knowledge base and found this information which might be useful:",
        "results": [
          {
            "title": null,
            "body": "You can override the scheduled temperature by moving the bubble on the temperature slider up or down. The blue number represents the cool set point; the orange number represents the heat set point. The new desired temperature will be the set point used for the Hold. The duration of the Hold is the last configured value (the default value is Until I change it, meaning it keeps the value indefinitely, until you choose to revert to the schedule or change it). You can adjust the default Hold time in the Preferences menu (page 21). To cancel the current Hold, touch the Hold message box displayed on the Home screen. You can touch the box anywhere and not just the X displayed on the box.",
            "url": null,
            "id": "3a5efee70d8cc9d70e2b94d22c15e2d1_24",
            "result_metadata": {
              "confidence": 0.3056213337457547,
              "score": 7.38697695
            },
            "highlight": {
              "text": [
                "You can override the <em>scheduled</em> temperature by moving the bubble on the temperature slider up or down. The blue number represents the cool <em>set</em> point; the orange number represents the heat <em>set</em> point. The new desired temperature will be the <em>set</em> point used for the Hold.",
                "The duration of the Hold is the last configured value (the default value is Until <em>I</em> change it, meaning it keeps the value indefinitely, until you choose to revert to the <em>schedule</em> or change it). You can adjust the default Hold time in the Preferences menu (page 21). To cancel the current Hold, touch the Hold message box displayed on the Home screen."
              ],
              "body": [
                "You can override the <em>scheduled</em> temperature by moving the bubble on the temperature slider up or down. The blue number represents the cool <em>set</em> point; the orange number represents the heat <em>set</em> point. The new desired temperature will be the <em>set</em> point used for the Hold.",
                "The duration of the Hold is the last configured value (the default value is Until <em>I</em> change it, meaning it keeps the value indefinitely, until you choose to revert to the <em>schedule</em> or change it). You can adjust the default Hold time in the Preferences menu (page 21). To cancel the current Hold, touch the Hold message box displayed on the Home screen."
              ]
            }
          },
```

# Learn more

* **Artificial Intelligence Code Patterns**: Enjoyed this Code Pattern? Check out our other [AI Code Patterns](https://developer.ibm.com/technologies/artificial-intelligence/)
* **AI and Data Code Pattern Playlist**: Bookmark our [playlist](https://www.youtube.com/playlist?list=PLzUbsvIyrNfknNewObx5N7uGZ5FKH0Fde) with all of our Code Pattern videos
* **With Watson**: Want to take your Watson app to the next level? Looking to utilize Watson Brand assets? [Join the With Watson program](https://www.ibm.com/watson/with-watson/) to leverage exclusive brand, marketing, and tech resources to amplify and accelerate your Watson embedded commercial solution.

# License

This code pattern is licensed under the Apache License, Version 2. Separate third-party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1](https://developercertificate.org/) and the [Apache License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache License FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)

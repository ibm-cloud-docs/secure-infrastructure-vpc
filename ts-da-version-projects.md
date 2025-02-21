---

copyright:
  years: 2023
lastupdated: "2025-02-21"

keywords:

subcollection: secure-infrastructure-vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How do I choose an older DA version after selecting a later one in Projects config UI?
{: #ts-da-version-projects}
{: troubleshoot}

Unable to select older version of deployable architure in Projects config UI.
{: shortdesc}

In the IBM Projects config UI, you have selected a specific version of the deployable architecure to update to and clicked 'Save', however you now wish to choose and older version but the version dropdown is greyed out.
{: tsSymptoms}

After choosing a new version in the Projects config UI, the version becomes locked while config changes, or validation occurs on it.
{: tsCauses}

Providing you have not proceeding with a deploy of the new version, you can change the version in the dropdown box by following these steps:
    - Locate the menu icon (three-dot symbol) on the top right of the projects config page and click it.
    - Choose to 'Discard edits'.
    - Click 'Discard' to proceed.
    - The version dropdown box becomes active again and a different version can be selected.
{: tsResolve}

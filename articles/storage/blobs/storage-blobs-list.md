---
title: Auflisten von Blobs mit .NET – Azure Storage
description: Hier erfahren Sie, wie Sie Blobs in einem Container in Ihrem Azure Storage-Konto mithilfe der .NET-Clientbibliothek auflisten. Codebeispiele veranschaulichen, wie Blobs in einer flachen Auflistung angezeigt werden oder wie Blobs hierarchisch aufgelistet werden, als wären Sie in Verzeichnissen oder Ordnern organisiert.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 06/05/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 81eab2f3c5748c4edf7e09e5147a178a08bc546b
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88185705"
---
# <a name="list-blobs-with-net"></a>Auflisten von Blobs mit .NET

Wenn Sie Blobs über Ihren Code auflisten, können Sie eine Reihe von Optionen angeben, um zu steuern, wie Ergebnisse von Azure Storage zurückgegeben werden. Sie können die Anzahl der Ergebnisse festlegen, die in den einzelnen Ergebnissätzen zurückgegeben werden sollen, und dann die nachfolgenden Sätze abrufen. Sie können ein Präfix angeben, um Blobs zurückzugeben, deren Namen mit dem jeweiligen Zeichen oder der Zeichenfolge beginnen. Sie können Blobs auch in einer flachen Auflistungsstruktur anzeigen oder hierarchisch auflisten. Bei einer hierarchischen Auflistung werden Blobs so zurückgegeben, als wären sie in Ordnern organisiert.

In diesem Artikel wird beschrieben, wie Blobs mithilfe der [Azure Storage-Clientbibliothek für .NET](/dotnet/api/overview/azure/storage?view=azure-dotnet) aufgelistet werden.  

## <a name="understand-blob-listing-options"></a>Grundlegendes zu den Optionen für das Auflisten von Blobs

Rufen Sie zum Auflisten der Blobs in einem Speicherkonto eine der folgenden Methoden auf:

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

- [BlobContainerClient.GetBlobs](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobs?view=azure-dotnet)
- [BlobContainerClient.GetBlobsAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsasync?view=azure-dotnet)
- [BlobContainerClient.GetBlobsByHierarchy](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchy?view=azure-dotnet)
- [BlobContainerClient.GetBlobsByHierarchyAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchyasync?view=azure-dotnet)

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

- [CloudBlobClient.ListBlobs](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobs)
- [CloudBlobClient.ListBlobsSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobssegmented)
- [CloudBlobClient.ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobssegmentedasync)

Rufen Sie zum Auflisten der Blobs in einem Container eine der folgenden Methoden auf:

- [CloudBlobContainer.ListBlobs](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobs)
- [CloudBlobContainer.ListBlobsSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmented)
- [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmentedasync)

Die Überladungen dieser Methoden bieten zusätzliche Optionen zum Steuern, wie Blobs durch den Auflistungsvorgang zurückgegeben werden. Diese Optionen sind in den folgenden Abschnitten beschrieben.

---

### <a name="manage-how-many-results-are-returned"></a>Festlegen der Anzahl der zurückgegebenen Ergebnisse

Standardmäßig gibt ein Auflistungsvorgang bis zu 5000 Ergebnisse in einem Durchgang zurück, Sie können jedoch die Anzahl der Ergebnisse angeben, die von jedem Auflistungsvorgang zurückgegeben werden soll. Die Beispiele in diesem Artikel veranschaulichen dies.

Wenn ein Auflistungsvorgang mehr als 5000 Blobs zurückgibt oder die Anzahl der verfügbaren Blobs die von Ihnen angegebene Anzahl überschreitet, gibt Azure Storage ein *Fortsetzungstoken* mit der Liste der Blobs zurück. Ein Fortsetzungstoken ist ein nicht transparenter Wert, den Sie verwenden können, um den nächsten Satz von Ergebnissen aus Azure Storage abzurufen.

Überprüfen Sie im Code den Wert des Fortsetzungstokens, um zu bestimmen, ob er NULL ist. Wenn das Fortsetzungstoken NULL ist, ist der Satz der Ergebnisse vollständig. Wenn das Fortsetzungstoken nicht NULL ist, rufen Sie den Auflistungsvorgang erneut auf, und übergeben Sie das Fortsetzungstoken, um den nächsten Ergebnissatz so oft abzurufen, bis das Fortsetzungstoken NULL ist.

### <a name="filter-results-with-a-prefix"></a>Filtern von Ergebnissen mit einem Präfix

Um die Liste der Container zu filtern, geben Sie für den `prefix`-Parameter eine Zeichenfolge an. Die Präfixzeichenfolge kann ein oder mehrere Zeichen enthalten. Dann gibt Azure Storage nur die Blobs zurück, deren Namen mit diesem Präfix beginnen.

### <a name="return-metadata"></a>Zurückgeben von Metadaten

Sie können zusammen mit den Ergebnissen Blobmetadaten zurückgeben. 

- Wenn Sie das .NET v12 SDK verwenden, geben Sie den Wert **Metadaten** für die [BlobTraits](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.models.blobtraits?view=azure-dotnet)-Enumeration an.

- Wenn Sie das .NET v11 SDK verwenden, geben Sie für die Enumeration [BlobListingDetails](/dotnet/api/microsoft.azure.storage.blob.bloblistingdetails) den Wert **Metadata** an. Weil Azure Storage Metadaten für jedes zurückgegebene Blob enthält, müssen Sie in diesem Kontext keine der **FetchAttributes**-Methoden aufrufen, um die Blobmetadaten abzurufen.

### <a name="flat-listing-versus-hierarchical-listing"></a>Flache Auflistung und hierarchische Auflistung im Vergleich

Blobs in Azure Storage sind in einem flachen Paradigma organisiert statt in einem hierarchischen Paradigma (wie ein klassisches Dateisystem). Sie können Blobs jedoch in *virtuellen Verzeichnissen* organisieren, um eine Ordnerstruktur zu imitieren. Ein virtuelles Verzeichnis bildet einen Teil des Blobnamens und wird durch das Trennzeichen angezeigt.

Wenn Sie also Blobs in virtuellen Verzeichnissen organisieren möchten, verwenden Sie ein Trennzeichen im Blobnamen. Das Standardtrennzeichen ist ein Schrägstrich (/), doch können Sie ein beliebiges Zeichen als Trennzeichen angeben.

Wenn Sie Ihre Blobs mithilfe eines Trennzeichens benennen, können Sie sie hierarchisch auflisten. Bei einem hierarchischen Auflistungsvorgang gibt Azure Storage alle virtuellen Verzeichnisse und Blobs unter dem übergeordneten Objekt zurück. Sie können den Auflistungsvorgang rekursiv aufrufen, um die Hierarchie zu durchlaufen – ähnlich wie beim programmgesteuerten Durchlaufen eines klassischen Dateisystems.

## <a name="use-a-flat-listing"></a>Verwenden einer flachen Auflistung

Ein Auflistungsvorgang gibt Blobs standardmäßig in einer flachen Auflistung zurück. In einer flachen Auflistung werden Blobs nicht nach virtuellem Verzeichnis organisiert.

Im folgenden Beispiel werden die Blobs im angegebenen Container mithilfe einer flachen Auflistung aufgeführt, wobei eine optionale Segmentgröße angegeben und der Blobname in ein Konsolenfenster geschrieben wird.

Wenn Sie für Ihr Konto das Feature für hierarchische Namespaces aktiviert haben, sind Verzeichnisse nicht virtuell. Vielmehr handelt es sich um konkrete, unabhängige Objekte. Daher werden Verzeichnisse in der Liste als Blobs mit der Länge 0 (null) angezeigt.

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/CRUD.cs" id="Snippet_ListBlobsFlatListing":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

```csharp
private static async Task ListBlobsFlatListingAsync(CloudBlobContainer container, int? segmentSize)
{
    BlobContinuationToken continuationToken = null;
    CloudBlob blob;

    try
    {
        // Call the listing operation and enumerate the result segment.
        // When the continuation token is null, the last segment has been returned
        // and execution can exit the loop.
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(string.Empty,
                true, BlobListingDetails.Metadata, segmentSize, continuationToken, null, null);

            foreach (var blobItem in resultSegment.Results)
            {
                blob = (CloudBlob)blobItem;

                // Write out some blob properties.
                Console.WriteLine("Blob name: {0}", blob.Name);
            }

            Console.WriteLine();

           // Get the continuation token and loop until it is null.
           continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

---

Die Beispielausgabe sieht ähnlich wie hier aus:

```
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

## <a name="use-a-hierarchical-listing"></a>Verwenden einer hierarchischen Auflistung

Wenn Sie einen Auflistungsvorgang hierarchisch aufrufen, gibt Azure Storage die virtuellen Verzeichnisse und Blobs auf der ersten Hiearchieebene zurück. Die Eigenschaft [Prefix](/dotnet/api/microsoft.azure.storage.blob.cloudblobdirectory.prefix) der einzelnen virtuellen Verzeichnisse ist so festgelegt, dass Sie das Präfix in einem rekursiven Aufruf übergeben können, um das nächste Verzeichnis abzurufen.

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

Um Blobs hierarchisch aufzulisten, müssen Sie die Methode [BlobContainerClient.GetBlobsByHierarchy](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchy?view=azure-dotnet) oder [BlobContainerClient.GetBlobsByHierarchyAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchyasync?view=azure-dotnet) aufrufen.

Im folgenden Beispiel werden die Blobs im angegebenen Container mithilfe einer hierarchischen Auflistung aufgeführt, wobei eine optionale Segmentgröße angegeben und der Blobname in das Konsolenfenster geschrieben wird.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/CRUD.cs" id="Snippet_ListBlobsHierarchicalListing":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

Wenn Blobs hierarchisch aufgelistet werden sollen, legen Sie den Parameter `useFlatBlobListing` der Auflistungsmethode auf **false** fest.

Im folgenden Beispiel werden die Blobs im angegebenen Container mithilfe einer flachen Auflistung aufgeführt, wobei eine optionale Segmentgröße angegeben und der Blobname in das Konsolenfenster geschrieben wird.

```csharp
private static async Task ListBlobsHierarchicalListingAsync(CloudBlobContainer container, string prefix)
{
    CloudBlobDirectory dir;
    CloudBlob blob;
    BlobContinuationToken continuationToken = null;

    try
    {
        // Call the listing operation and enumerate the result segment.
        // When the continuation token is null, the last segment has been returned and
        // execution can exit the loop.
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(prefix,
                false, BlobListingDetails.Metadata, null, continuationToken, null, null);
            foreach (var blobItem in resultSegment.Results)
            {
                // A hierarchical listing may return both virtual directories and blobs.
                if (blobItem is CloudBlobDirectory)
                {
                    dir = (CloudBlobDirectory)blobItem;

                    // Write out the prefix of the virtual directory.
                    Console.WriteLine("Virtual directory prefix: {0}", dir.Prefix);

                    // Call recursively with the prefix to traverse the virtual directory.
                    await ListBlobsHierarchicalListingAsync(container, dir.Prefix);
                }
                else
                {
                    // Write out the name of the blob.
                    blob = (CloudBlob)blobItem;

                    Console.WriteLine("Blob name: {0}", blob.Name);
                }
                Console.WriteLine();
            }

            // Get the continuation token and loop until it is null.
            continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

---

Die Beispielausgabe sieht ähnlich wie hier aus:

```
Virtual directory prefix: FolderA/
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt

Virtual directory prefix: FolderA/FolderB/
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt

Virtual directory prefix: FolderA/FolderB/FolderC/
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

> [!NOTE]
> Blobmomentaufnahmen können nicht in einem hierarchischen Auflistungsvorgang aufgeführt werden.

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Auflisten von Blobs](/rest/api/storageservices/list-blobs)
- [Aufzählen von Blobressourcen](/rest/api/storageservices/enumerating-blob-resources)

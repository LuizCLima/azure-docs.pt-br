---
title: Referência rápida para comandos de trabalhos de importação da Ferramenta de Importação/Exportação do Azure | Microsoft Docs
description: Referência de comandos da Ferramenta de Importação/Exportação do Azure para comandos de trabalho de importação usados com frequência.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 7d21ab42188d5b8d6cb8c179a28d1b5bee822f05
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55453579"
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Referência rápida de comandos usados com frequência para trabalhos de importação

Este artigo fornece uma referência rápida de alguns comandos usados com frequência. Para obter o uso detalhado, consulte [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import.md) (Preparando os discos rígidos para um trabalho de importação).

## <a name="first-session"></a>Primeira sessão

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

## <a name="second-session"></a>Segunda sessão

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /DataSet:dataset-2.csv
```

## <a name="abort-latest-session"></a>Anular a sessão mais recente

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /AbortSession
```

## <a name="resume-latest-interrupted-session"></a>Retomar a sessão interrompida mais recente

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /ResumeSession
```

## <a name="add-drives-to-latest-session"></a>Adicionar unidades à sessão mais recente

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /AdditionalDriveSet:driveset-2.csv
```

## <a name="next-steps"></a>Próximas etapas

* [Fluxo de trabalho de exemplo para preparar discos rígidos para um trabalho de importação](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)

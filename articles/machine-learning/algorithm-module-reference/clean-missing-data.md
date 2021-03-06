---
title: 清除遺漏的資料：模組參考
titleSuffix: Azure Machine Learning
description: 瞭解如何使用 Azure Machine Learning 中的「清除遺漏的資料」模組來移除、取代或推斷遺漏值。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: 5b7943b2026d640ae7e5d119e165bd752ae2fe7f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "90898818"
---
# <a name="clean-missing-data-module"></a>清除遺漏的資料模組

本文描述 Azure Machine Learning 設計工具中的模組。

您可以使用此模組來移除、取代或推斷遺漏值。 

資料科學家通常會檢查遺漏值的資料，然後執行各種作業來修正資料或插入新值。 這類清除作業的目標是要防止在定型模型時可能發生的資料遺失所造成的問題。 

此模組支援多個類型的「清除」遺漏值的作業，包括：

+ 以預留位置、mean 或其他值取代遺漏值
+ 完全移除具有遺漏值的資料列和資料行
+ 根據統計方法推斷值


使用此模組並不會變更您的源資料集。 相反地，它會在您的工作區中建立新的資料集，供您在後續的工作流程中使用。 您也可以儲存新的、已清除的資料集以供重複使用。

此模組也會輸出用來清除遺漏值的轉換定義。 您可以使用「套用 [轉換](./apply-transformation.md) 」模組，在具有相同架構的其他資料集上重複使用這個轉換。  

## <a name="how-to-use-clean-missing-data"></a>如何使用清除遺漏的資料

此課程模組可讓您定義清除作業。 您也可以儲存清除作業，讓您稍後可以將它套用至新的資料。 請參閱下列有關如何建立和儲存清理程式的章節： 
 
+ [取代遺漏值](#replace-missing-values)
  
+ [將清除轉換套用至新的資料](#apply-a-saved-cleaning-operation-to-new-data)
 
> [!IMPORTANT]
> 您用來處理遺漏值的清除方法，可能會大幅影響您的結果。 建議您試驗不同的方法。 請考慮使用特定方法的理由和結果品質。

### <a name="replace-missing-values"></a>取代遺漏值  

每次您將 [  [清除遺漏的資料](./clean-missing-data.md) ] 模組套用至一組資料時，就會對您選取的所有資料行套用相同的清除作業。 因此，如果您需要使用不同的方法來清除不同的資料行，請使用不同的模組實例。

1.  將 [ [清除遺漏的資料](./clean-missing-data.md) ] 模組新增至您的管線，並連接具有遺漏值的資料集。  
  
2.  針對 **要清除**的資料行，選擇包含您想要變更之遺漏值的資料行。 您可以選擇多個資料行，但必須在所有選取的資料行中使用相同的取代方法。 因此，您通常需要分開清除字串資料行和數值資料行。

    例如，若要檢查所有數值資料行中的遺漏值：

    1. 選取 [ **清除遺漏的資料** ] 模組，然後按一下模組右面板中的 [ **編輯資料行** ]。

    3. 針對 [ **包含**]，從下拉式清單中選取 [資料 **行類型** ]，然後選取 [ **數值**]。 
  
    您所選擇的任何清除或取代方法，都必須適用于選取範圍中的 **所有** 資料行。 如果任何資料行中的資料與指定的作業不相容，模組會傳回錯誤並停止管線。
  
3.  針對 [ **最小遺漏值比率**]，指定執行作業所需的遺漏值的最小數目。  
  
    您可以使用此選項搭配 **最大遺漏值比率** ，來定義在資料集上執行清除作業的條件。 如果遺漏值的資料列太多或太少，就無法執行此作業。 
  
    您輸入的數位表示資料行中所有值的遺漏值 **比率** 。 根據預設， **最小遺漏值比率** 屬性會設定為0。 這表示即使只有一個遺漏值，也會清除遺漏值。 

    > [!WARNING]
    > 每個資料行都必須符合此條件，才能套用指定的作業。 例如，假設您選取三個數據行，然後將遺漏值的最小比率設為 .2 (20% ) ，但只有一個資料行實際上有20% 的遺漏值。 在此情況下，清除作業只會套用至具有超過20% 遺漏值的資料行。 因此，其他資料行則不會變更。
    > 
    > 如果您對遺漏值是否已變更有所疑慮，請選取 **[產生遺漏值指標資料行]** 選項。 資料集會附加資料行，以指出每個資料行是否符合最小和最大範圍的指定準則。  
  
4. 針對 [ **最大遺漏值比率**]，指定可針對要執行之作業顯示的遺漏值的最大數目。   
  
    例如，您可能只想要在有30% 或更少的資料列包含遺漏值時，才執行遺漏值替代，但如果有超過30% 的資料列有遺漏值，則將這些值保留原狀。  
  
    您可以定義數目為遺漏值，以資料行中的所有值的比例。 根據預設， **最大遺漏值比例** 會設定為1。 這表示即使遺漏資料行中值的100%，也會清除遺漏值。  
  
   
  
5. 針對 [ **清除模式]**，選取下列其中一個選項來取代或移除遺漏值：  
  
  
    + **自訂替代值**：使用此選項來指定預留位置值 (例如套用至所有遺漏值的0或 NA) 。 您指定為取代的值必須與資料行的資料類型相容。
  
    + **以 Mean 取代**：計算資料行的平均值，並使用 mean 作為資料行中每個遺漏值的取代值。  
  
        只適用於具有整數、 Double、 資料行或 Boolean 資料型別。  
  
    + **取代為中間**值：計算資料行的中位數值，並使用中值做為資料行中任何遺漏值的取代值。  
  
        只適用於具有整數、 Double、 資料行或 Boolean 資料型別。 
  
    + **取代為模式**：計算資料行的模式，並使用此模式作為資料行中每個遺漏值的取代值。  
  
        適用於 Integer、 Double、 布林值或 Categorical 資料類型的資料行。 
  
    + **移除整個資料列**：完全移除資料集內有一個或多個遺漏值的任何資料列。 如果遺漏值可視為隨機遺漏，此方法非常有用。  
  
    + **移除整個資料行**：完全移除資料集裡有一或多個遺漏值的任何資料行。  
  
    
  
6. 如果您已選取 [**自訂替換值**] 選項，則可以使用選項**取代值**。 輸入新的值，做為資料行中所有遺漏值的取代值。  
  
    請注意，您只能在具有整數、雙精確度、布林值或字串的資料行中使用此選項。
  
7. **產生遺漏值指標資料行**：如果您想要輸出資料行中的值是否符合遺漏值清除的準則，請選取此選項。 當您要設定新的清除作業，並想要確保它如設計運作時，此選項特別有用。
  
8. 提交管線。

### <a name="results"></a>結果

模組會傳回兩個輸出：  

-   已**清除的資料集**：包含所選資料行的資料集，如果您選取了該選項，則會以指定的方式處理遺漏值以及指標資料行。  

    未選取要清除的資料行也會「通過」。  
  
-  **清除轉換**：用於清除的資料轉換，可儲存在您的工作區中，之後再套用至新的資料。

### <a name="apply-a-saved-cleaning-operation-to-new-data"></a>將已儲存的清理作業套用至新的資料  

如果您需要經常重複清除作業，建議您將資料清理的配方儲存為 *轉換*，以重複使用相同的資料集。 如果您經常必須重新匯入，然後清除具有相同架構的資料，則儲存清除轉換特別有用。  
      
1.  將 [套用 [轉換](./apply-transformation.md) ] 模組新增至您的管線。  
  
2.  新增您想要清除的資料集，並將資料集連接至右手邊的輸入埠。  
  
3.  在設計工具的左窗格中，展開 [ **轉換** ] 群組。 找出已儲存的轉換，然後將它拖曳到管線中。  

4.  將儲存的轉換連接到 [套用 [轉換](./apply-transformation.md)] 的左側輸入埠。 

    當您套用儲存的轉換時，無法選取要套用轉換的資料行。 這是因為轉換已經過定義，並且會自動套用至原始作業中所指定的資料行。

    不過，假設您在數值資料行的子集上建立了轉換。 您可以將此轉換套用至混合資料行類型的資料集，而不會引發錯誤，因為遺漏值只會在相符的數值資料行中變更。

6.  提交管線。  

## <a name="next-steps"></a>後續步驟

請參閱 Azure Machine Learning 的[可用模組集](module-reference.md)。 
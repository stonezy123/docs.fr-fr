---
title: "Comment : définir une icône pour un bouton de barre d'outils"
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- toolbars [Windows Forms], adding icons to buttons
- buttons [Windows Forms], icons
- examples [Windows Forms], toolbars
- images [Windows Forms], toolbar buttons
- icons [Windows Forms], toolbar buttons
- ToolBar control [Windows Forms], adding icons to buttons
ms.assetid: 84db98b4-8566-49ce-b2c8-1fd66a5eb3a0
ms.openlocfilehash: 84c67c7d2584390ba3e48cb83820c65c6bb45d1f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79182208"
---
# <a name="how-to-define-an-icon-for-a-toolbar-button"></a>Comment : définir une icône pour un bouton de barre d'outils
> [!NOTE]
> Le contrôle <xref:System.Windows.Forms.ToolStrip> remplace le contrôle <xref:System.Windows.Forms.ToolBar> et lui ajoute des fonctionnalités ; toutefois, le contrôle <xref:System.Windows.Forms.ToolBar> est conservé pour la compatibilité descendante et l'utilisation future si tel est votre choix.  
  
 <xref:System.Windows.Forms.ToolBar>les boutons sont capables d’afficher des icônes à l’intérieur d’eux pour une identification facile par les utilisateurs. Ceci est réalisé en ajoutant des images au composant <xref:System.Windows.Forms.ImageList> de <xref:System.Windows.Forms.ToolBar> [composant ImageList,](imagelist-component-windows-forms.md) puis en associant le composant au contrôle.  
  
### <a name="to-set-an-icon-for-a-toolbar-button-programmatically"></a>Définir une icône pour un bouton de barre d’outils programmatiquement  
  
1. Dans une procédure, instantané <xref:System.Windows.Forms.ImageList> d’un composant et d’un <xref:System.Windows.Forms.ToolBar> contrôle.  
  
2. Dans la même procédure, assignez une image au <xref:System.Windows.Forms.ImageList> composant.  
  
3. Dans la même procédure, assigner le <xref:System.Windows.Forms.ImageList> contrôle au <xref:System.Windows.Forms.ToolBar> contrôle et attribuer la <xref:System.Windows.Forms.ToolBarButton.ImageIndex%2A> propriété des boutons individuels de la barre d’outils.  
  
     Dans l’exemple de code suivant, le chemin défini pour l’emplacement de l’image est le dossier **Mes Documents.** Cela est fait, parce que vous pouvez supposer que la plupart des ordinateurs exécutant le système d’exploitation Windows inclura ce répertoire. Cela permet également aux utilisateurs avec des niveaux d’accès minimum au système d’exécuter l’application en toute sécurité. L’exemple ci-dessous suppose <xref:System.Windows.Forms.PictureBox> un formulaire avec un contrôle déjà ajouté.  
  
     En suivant les étapes ci-dessus, vous devriez avoir un code écrit similaire à celui affiché ci-dessous.  
  
    ```vb  
    Public Sub InitializeMyToolBar()  
    ' Instantiate an ImageList component and a ToolBar control.  
       Dim ToolBar1 as New ToolBar  
       Dim ImageList1 as New ImageList  
    ' Assign an image to the ImageList component.  
    ' You should replace the bold image  
    ' in the sample below with an icon of your own choosing.  
       Dim myImage As System.Drawing.Image = _
          Image.FromFile Image.FromFile _  
          (System.Environment.GetFolderPath _  
          (System.Environment.SpecialFolder.Personal) _  
          & "\Image.gif")  
       ImageList1.Images.Add(myImage)  
    ' Create a ToolBarButton.  
       Dim ToolBarButton1 As New ToolBarButton()  
    ' Add the ToolBarButton to the ToolBar.  
       ToolBar1.Buttons.Add(toolBarButton1)  
    ' Assign an ImageList to the ToolBar.  
       ToolBar1.ImageList = ImageList1  
    ' Assign the ImageIndex property of the ToolBarButton.  
       ToolBarButton1.ImageIndex = 0  
    End Sub  
    ```  
  
    ```csharp  
    public void InitializeMyToolBar()  
    {  
       // Instantiate an ImageList component and a ToolBar control.  
       ToolBar toolBar1 = new  ToolBar();
       ImageList imageList1 = new ImageList();  
       // Assign an image to the ImageList component.  
       // You should replace the bold image
       // in the sample below with an icon of your own choosing.  
       // Note the escape character used (@) when specifying the path.  
       Image myImage = Image.FromFile  
       (System.Environment.GetFolderPath  
       (System.Environment.SpecialFolder.Personal)  
       + @"\Image.gif");  
       imageList1.Images.Add(myImage);  
       // Create a ToolBarButton.  
       ToolBarButton toolBarButton1 = new ToolBarButton();  
       // Add the ToolBarButton to the ToolBar.  
       toolBar1.Buttons.Add(toolBarButton1);  
       // Assign an ImageList to the ToolBar.  
       toolBar1.ImageList = imageList1;  
       // Assign ImageIndex property of the ToolBarButton.  
       toolBarButton1.ImageIndex = 0;  
    }  
    ```  
  
    ```cpp  
    public:  
       void InitializeMyToolBar()  
       {  
          // Instantiate an ImageList component and a ToolBar control.  
          ToolBar ^ toolBar1 = gcnew  ToolBar();
          ImageList ^ imageList1 = gcnew ImageList();  
          // Assign an image to the ImageList component.  
          // You should replace the bold image
          // in the sample below with an icon of your own choosing.  
          Image ^ myImage = Image::FromFile(String::Concat  
             (System::Environment::GetFolderPath  
             (System::Environment::SpecialFolder::Personal),  
             "\\Image.gif"));  
          imageList1->Images->Add(myImage);  
          // Create a ToolBarButton.  
          ToolBarButton ^ toolBarButton1 = gcnew ToolBarButton();  
          // Add the ToolBarButton to the ToolBar.  
          toolBar1->Buttons->Add(toolBarButton1);  
          // Assign an ImageList to the ToolBar.  
          toolBar1->ImageList = imageList1;  
          // Assign ImageIndex property of the ToolBarButton.  
          toolBarButton1->ImageIndex = 0;  
       }  
    ```  
  
## <a name="see-also"></a>Voir aussi

- <xref:System.Windows.Forms.ToolBar>
- [Guide pratique pour déclencher des événements de menu pour les boutons de barre d'outils](how-to-trigger-menu-events-for-toolbar-buttons.md)
- [ToolBar, contrôle](toolbar-control-windows-forms.md)
- [ImageList, composant](imagelist-component-windows-forms.md)

# CustomEditor
### When do i need to use a custom editor
If you need to display special things in your script inspector or display specific variables, but separated by, an enum for example, the **Custom inspector** is for you

### Example script

```
#if UNITY_EDITOR                                    
// This #if execute itself only when you're in the Unity Editor, it's not compiled when you build your game.

using UnityEditor;   // Unity Editor API

[CanEditMultipleObjects]  
// Use it if you need to change values in multiple objects that have the same script.
[CustomEditor(typeof(ButtonCall))]   
// You say to your code : i create a custom editor for a script called "ButtonCall".
public class ButtonCallEditor : Editor  
// Use a class from "Editor" and not from "Mono behaviour".  [This is our Editor script]
{
    ButtonCall cs_Script;  
    // You create a variable that will contain your script with his functions.
    private void OnEnable()
    {
        cs_Script = (ButtonCall)target;  
        // When your script is enable you cast your function script to your "cs_script".
    }

    public override void OnInspectorGUI()  
    // This overide the function that normally display your "public/[SerializeField]".
    {
    
        // if you only need to add info in your inspector and not recreate it you can add :
        // base.OnInspectorGUI();


        Undo.RecordObject(target, "ButtonCall");    
        // This function allows you to save the data you set in the inspector
        (naturally, it do not set your script "dirty" when you change a value).

        cs_Script.TypeOfButton = (ButtonType)EditorGUILayout.EnumPopup("Button Type", cs_Script.TypeOfButton); 
        // Show an enum popup on the inspector that contain our TypeOfButton from the function script.

        switch (cs_Script.TypeOfButton)  
        // This switch allows us to modify what will be displayed linked to our choice in the enum.
        {
            case ButtonType.Tab:
                cs_Script.ButtonIndex = EditorGUILayout.IntField("Button Index", cs_Script.ButtonIndex);  // An int field.
                break;
            case ButtonType.AddressBookNote:
                cs_Script.ButtonIndex = EditorGUILayout.IntField("Page Index", cs_Script.ButtonIndex);
                break;
            default:
                break;
        }
    }
}
#endif 
```
The entire script can be downloaded with the repository (**CustoEditor.cs**)

### More function for inspector
if you need specific functions like a button in your inspector, you can use this type of thing 
```
if (GUILayout.Button("Your Button Name"))
        {
            // Call your function
        }
```

#### Unity Doc
if you need specific inspector field (Object field, int field...) the [Unity Doc](https://docs.unity3d.com/ScriptReference/EditorGUILayout.html) is here for you !

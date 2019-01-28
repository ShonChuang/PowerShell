# 自訂Help

```text
//放入關鍵字註解
<#
    .SYNOPSIS
      Displays a list of WMI Classes based upon a search criteria
    .EXAMPLE
     Get-WmiClasses -class disk -ns rootcimv2"
     This command finds wmi classes that contain the word disk. The
     classes returned are from the rootcimv2 namespace.
#>
```



**Table 1 Function Help Tags and Meanings**

| **Help tag name** | **Help tag description** |
| :--- | :--- |
| .Synopsis | A very brief description of the function. It begins with a verb and tells the user what the function does. It does not include the function name or how the function works. The function synopsis appears in the SYNOPSIS field of all help views. |
| .Description | Two or three full sentences that briefly list everything that the function can do. The description begins with "The &lt;function name&gt; function…." If the function can get multiple objects or take multiple inputs, use plural nouns in the description. The description appears in the DESCRIPTION field of all Help views. |
| .Parameter | Brief and thorough. Describe what the function does when the parameter is used. And what legal values are for the parameter. The parameter appears in the PARAMETERS field only in Detailed and Full Help views. |
| .Example | Illustrate use of function with all its parameters. First example is simplest with only the required parameters. Last example is most complex and should incorporate pipelining if appropriate. The example appears only in the EXAMPLES field in the Example, Detailed, and Full Help views. |
| .Inputs | Lists the .NET Framework classes of objects the function will accept as input. There is no limit to the number of input classes you may list. The inputs Help tag appears only in the INPUTS field in the Full Help view. |
| .Outputs | Lists the .NET Framework classes of objects the function will emit as output. There is no limit to the number of output classes you may list. The outputs Help tag appears in the OUTPUTS field only in the Full Help view. |
| .Notes | Provides a place to list information that does not fit easily into the other sections. This can be special requirements required by the function, as well as author, title, version, and other information. The notes Help tag appear in the NOTES field only in the Full help view. |
| .Link | Provides links to other Help topics and Internet Web sites of interest. Because these links appear in a command window, they are not direct links. There is no limit to the number of links you may provide. The links appear in the RELATED LINKS field in all Help views. |


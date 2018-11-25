function init([string]$Install, [string]$Output, [string]$PgsFile, [string]$Option){
    [Xml]$Pom = Get-Content ".\pom.xml"
    $Ins = $Pom.project.property.arInstallDirectory
    $Out = $Pom.project.build.plugins.plugin.config.arOutputDirectory
    $Pgs = $Pom.project.build.plugins.plugin.config.pgsFile
    $Opt = $Pom.project.build.plugins.plugin.config.arOptions
    
    $InsDelFlag = 0
    $OutDelFlag = 0
    $OptDelFlag = 0
    
    if($Install -eq ""){
        $Pom.project.property.RemoveChild($Pom.getElementsByTagName("arInstallDirectory")[0])
        $InsDelFlag = 1
    }else{
        $Pom.project.property.arInstallDirectory = $Install
    }
    if($Output -eq ""){
        $Pom.project.build.plugins.plugin.config.RemoveChild($Pom.getElementsByTagName("arOutputDirectory")[0])
        $OutDelFlag = 1
    }else{
        $Pom.project.build.plugins.plugin.config.arOutputDirectory = $Output
    }
    if($Option -eq ""){
        $Pom.project.build.plugins.plugin.config.RemoveChild($Pom.getElementsByTagName("arOptions")[0])
        $OptDelFlag = 1
    }else{
        $Pom.project.build.plugins.plugin.config.arOptions.arOption = $Option
    }
    $Pom.get_InnerXml() | Out-File ".\pom.xml"
    
    $Pom.get_InnerXml() >> ".\test.log"
    
    if($InsDelFlag -eq 1){
        $InsNode = $Pom.CreateElement("arInstallDirectory")
        $Pom.getElementsByTagName("property").AppendChild($InsNode)
    }
    if($OutDelFlag -eq 1){
        $OutNode = $Pom.CreateElement("arOutputDirectory")
        $Pom.getElementsByTagName("config").AppendChild($OutNode)
    }
    if($OptDelFlag -eq 1){
        $OptNode = $Pom.CreateElement("arOptions")
        $OptNode.set_InnerXML("<arOption></arOption>")
        $Pom.getElementsByTagName("config").AppendChild($OptNode)
    }
    $Pom.get_InnerXml() | Out-File ".\pom.xml"
}


init "a" "b" "c" "d"
init "1" "" "3" ""
init "" "" "" ""

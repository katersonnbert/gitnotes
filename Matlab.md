# Matlab

## Cellfun
f = regexp(prop, '_', 'split');
fname = cellfun(@(x) strcat(upper(x(1)), x(2:end)), f, 'UniformOutput', 0);
fname = strcat(fname{:});

## Help

https://de.mathworks.com/help/matlab/matlab_prog/add-help-for-your-program.html

## varargin

https://de.mathworks.com/help/matlab/ref/varargin.html

## OOP in Matlab

https://de.mathworks.com/help/matlab/object-oriented-programming.html


## Adusting matlab color scheme

- [Undocumented matlab](http://undocumentedmatlab.com/blog/changing-system-preferences-programmatically)
- [Changing the color scheme](http://www.mikesoltys.com/2013/02/08/matlab-tip-change-the-color-scheme-to-be-easier-on-your-eyes/)
- [matlab-schemer](https://github.com/scottclowe/matlab-schemer)

Find out where the matlab user preferences file is kept:

- open matlab
- type in 'prefdir'
- copy path
- open file 'matlab.prf'
- paste in the following:


        Desktop.Font.Code=F0 17 Consolas
        Desktop.Font.Text=F0 17 Consolas
        
        Colors_M_Keywords=C-14114579
        Colors_M_Comments=C-16734719
        Colors_M_Strings=C-20124
        Colors_M_UnterminatedStrings=C-4210944
        Colors_M_SystemCommands=C-5941774
        Colors_M_Errors=C-65536
        Colors_HTML_HTMLLinks=C-10592257
        Colors_M_Warnings=C-27648
        
        ColorsBackground=C-16777216
        ColorsMLintAutoFixBackground=C-9223357
        ColorsText=C-1
        ColorsUseSystem=Bfalse
        ColorsUseMLintAutoFixBackground=Btrue
        
        Editor.VariableHighlighting.Automatic=Btrue
        Editor.VariableHighlighting.Color=C-11184786
        Editor.NonlocalVariableHighlighting=Btrue
        Editor.NonlocalVariableHighlighting.TextColor=C-16735351
        EditorCodepadHighVisible=Btrue
        EditorCodeBlockDividers=Btrue
        EditorRightTextLineVisible=Btrue
        EditorRightTextLimitLineWidth=I1
        Editorhighlight-lines=C-14408662
        Editorhighlight-caret-row-boolean=Btrue
        Editorhighlight-caret-row-boolean-color=C-12632257
        EditorRightTextLimitLineColor=C-5723992
        
        # TLC
        Editor.Language.TLC.Color.Colors_M_Keywords=C-14114579
        # C/C++
        Editor.Language.C.Color.preprocessor=C-16735351
        # VHDL
        Editor.Language.VHDL.Color.operator=C-16735351
        # Verilog
        Editor.Language.Verilog.Color.operator=C-16735351
        # XML
        Editor.Language.XML.Color.operator=C-1710454
        Editor.Language.XML.Color.doctype=C-6578958
        Editor.Language.XML.Color.pi-content=C-9868801






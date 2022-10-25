## [G-01] Use custom errors isntead of revert strings to save gas
File Holograph.sol: [165](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/Holograph.sol#L165)

File HolographOperator.sol: [241](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L241), [309](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L309), [350](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L350), [354](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L354), [368](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L368), [415](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L415), [446](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L446), [485](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L485), [591](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L591), [595](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L595), [728](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L595), [739](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L739), [756](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L756), [829](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L829), [839](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L839), [857](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857), [863](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L863), [881](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L881), [889](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L889), [903](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L903), [911](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L911), [915](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L915), [932](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L932)

File HolographFactory.sol: [144](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L144),[220](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L220),[228](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L228),[254](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L254)

File LayerZeroModule.sol: [159](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L159), [235](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L235)

File Holographer.sol: [148](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L148), [166](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L166)

File PA1D.sol: [159](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L159), [174](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L174), [190](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L190), [390](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L390), [411](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L411), [416](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L416), [435](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L435), [439](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L439), [460](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L460), [472](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L472), [477](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L460)


File HolographERC721.sol: [212](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L212), [224](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L224), [239](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L239), [258](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L258), [263](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263), [323](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263), [370-371](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L370-L371), [388](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L388), [404](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L404), [408](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L408), [419-421](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L419-L421), [458](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L458), [469](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L469), [484](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L484), [513](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L513), [622](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L622), [639](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L639), [689](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L689), [700](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L700), [729](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L729), [757](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L757), [762](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L762), [815-818](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L815-L818), [869-870](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L815-L818),  [906](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L906)

File HolographERC20.sol: [192](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L192), [204](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L204), [219](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L219), [241](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L241), [349](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L349),[365](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L365), [387](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L387), [400](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L400), [427](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L427), [445](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L445), [450](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L450), [469](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L469), [482](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L482), [505](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L505), [529](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L529), [539](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L539), [599](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L599), [620-621](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L620-L621), [627](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L627), [629](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L629),  [645](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L645), [684](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L684), [695-696](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L695-L696), [698](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L698),


File ERC721H.sol: [117](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L117), [123](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L123), [125](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L125), [147](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L147)

File ERC20H.sol: [117](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L117), [123](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L123),[125](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L125),  [147](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L147)

## [G-02]  require()/revert() strings longer than 32 bytes cost extra gas
File Holograph.sol: [165](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/Holograph.sol#L165)

File HolographOperator.sol: [241](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L241), [309](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L309), [350](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L350), [354](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L354), [368](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L368), [368](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L415), [446](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L446), [485](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L485), [591](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L591), [595](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L595), [728](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L595), [739](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L739), [739](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L739), [756](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L756),[829](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L829), [839](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L839), [857](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857), [863](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L863), [881](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L881), [889](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L889), [903](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L903), [911](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L911), [915](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L915), [932](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L932)

File HolographFactory.sol: [144](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L144),[220](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L220),[228](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L228),[254](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L254)

File LayerZeroModule.sol: [159](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L159), [235](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L235)

File Holographer.sol: [148](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L148), [166](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L166)

File PA1D.sol: [159](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L159), [174](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L174), [190](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L190), [390](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L390), [411](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L411), [416](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L416), [435](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L435), [439](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L439), [460](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L460), [472](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L472), [477](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L460)

File HolographERC721.sol: [212](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L212), [224](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L224), [239](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L239), [258](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L258), [263](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263), [323](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263), [370-371](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L370-L371), [388](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L388), [404](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L404), [408](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L408), [419-421](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L419-L421), [458](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L458), [469](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L469), [484](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L484), [513](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L513), [622](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L622), [639](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L639), [689](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L689), [700](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L700), [729](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L729), [757](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L757), [762](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L762), [764](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L764), [815-818](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L815-L818), [869-870](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L815-L818),  [906](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L906)

File HolographERC20.sol: [192](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L192), [204](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L204), [219](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L219), [241](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L241), [349](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L349),[365](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L365), [387](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L387), [400](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L400), [427](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L427), [445](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L445), [450](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L450), [452](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L452), [469](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L469), [482](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L482), [505](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L505), [529](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L529), [539](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L539), [599](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L599), [620-621](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L620-L621), [627](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L627), [629](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L629), [645](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L645), [653](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L653), [661](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L661), [665](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L665)

File ERC721H.sol: [117](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L117), [123](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L123), [125](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L125), [147](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L147)

File ERC20H.sol: [117](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L117), [123](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L123),[125](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L125),  [147](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L147)
## [G-03] X > 0 is less efficient than != 0 for unsigned integers
File HolographOperator.sol: [309](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L309), [350](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L350), [363](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L363), [398](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L398), [1126](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1126)



## [G-03] Optimizations with assembly
### [G-03.1] Use assembly for math (add, sub, mul, div)
File HolographOperator.sol: [1177](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1177)

File LayerZeroModule.sol: [293](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L293)

File PA1D.sol: [388](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L388), [395](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L395), [415](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L415), [438](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L438), [551](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L551), [553](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L553), [641](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L641), [643](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L643)


### [G-03.2] Use assembly to check for address(0)
File HolographOperator.sol: [333](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L333)


File PA1D.sol: [550](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L550), [560](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L550), [571](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L550), [593](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L593), [607](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L607), [627](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L627), [640](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L640), [657](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L657), [669](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L669)

File HolographERC721.sol: [419](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L419), [639](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L639), [657](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L657), [689](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L689), [816](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L816), [870](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L870), [895](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L895)

 File HolographERC20.sol: [620-621](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L620-L621), [627](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L627), [684](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L684), [695-696](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L695-L696)

## [G-03] X += Y or X -= Y costs more gas than  X = X + Y or X = X - Y  for state variables
File HolographOperator.sol: [378](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L378), [382](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L382), [834](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L834)

File HolographERC20.sol: [633](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L633), [702](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L702), [685](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L685), [686](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L686)

## [G-04] Using unchecked blocks to save gas
### [G-04.1] Use Add unchecked {} where the operands can not underflow/overflow because of a previous check
File HolographOperator.sol: [1174](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1174)

File PA1D.sol: [391](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L391)

### [G-04.2] Increments in for loop can be unchecked
The majority of Solidity for loops increment a uint256 variable that starts at 0. These increment operations never need to be checked for over/underflow because the variable will never reach the max number of uint256 (will run out of gas long before that happens). The default over/underflow check wastes gas in every iteration of virtually every for loop . eg.

File HolographOperator.sol: [781](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781), [871](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871)

File PA1D.sol: [307](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307), [323](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323), [340](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340), [356](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356), [394](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394), [414](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414), [432](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432), [437](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432), [454](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454), [474](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474)

File HolographERC721.sol: [357](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357), [716](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716)

File HolographERC20.sol: [564](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564)

## [G-05] ++i costs less gas compared to i++ or i += 1 in for loops (~5 gas per iteration)
++i costs less gas compared to i++ or i += 1 for unsigned integer, as pre-increment is cheaper

File HolographOperator.sol: [781](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781)

File PA1D.sol: [307](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307), [323](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323), [340](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340), [356](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356), [394](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394), [414](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414), [432](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432), [437](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432), [454](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454), [474](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474)

File HolographERC721.sol: [357](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357), [716](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716)

File HolographERC20.sol: [564](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564)

## [G-06] It costs more gas to initialize variables with their default value than letting the default value be applied
If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address...). Explicitly initializing it with its default value is an anti-pattern and wastes gas. Consider removing explicit initializations for default values.

File HolographOperator.sol: [781](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L781)

File PA1D.sol: [307](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L307), [323](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L323), [340](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L340), [356](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L356), [394](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L394), [414](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L414), [432](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432), [437](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432), [454](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454), [474](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474)

File HolographERC721.sol: [357](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357), [716](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L716)

File HolographERC20.sol: [564](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564)

## [G-07] Splitting require() statements that use && saves gas
File HolographOperator.sol: [857](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L857)

File Holographer.sol: [166](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L166)

File HolographERC721.sol: [263](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L263)

File HolographERC721.sol: [465-466](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L465-L466)

## [G-08] Empty blocks should be removed or emit something
Empty `receive()/fallback()` payable functions that are not used, can be removed to save deployment gas. The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting.

File HolographOperator.sol: [1209](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L1209)

File Holographer.sol: [223](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L223)

File HolographERC721.sol: [962](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L962)

File HolographERC20.sol: [251](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L251)

File ERC721H.sol [212](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L212)

File ERC20H.sol [212](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L212)
## [G-09] Cache storage values in memory to minimize SLOADs
cache `_operatorPods.length` File HolographOperator.sol: [867](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L867), [871](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L871)

## [G-10] Using calldata instead of memory for read-only arguments in external functions saves gas

File HolographBridge.sol: [162](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol#L162)

File HolographOperator.sol: [240](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol#L240)

File HolographFactory.sol: [145](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L145), [193-194](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol#L193-L194)

File LayerZeroModule.sol:  [158](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol#L158)

File Holographer.sol: [147](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L147)

File PA1D.sol: [173](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L173), [185](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L185)

File HolographERC721.sol: [238](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L238)

File HolographERC20.sol: [218](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L218)

File ERC721H.sol: [140](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol#L140)

File ERC20H.sol [140](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol#L140) 

## [G-11] USE ABI.ENCODEWITHSELECTOR INSTEAD OF ABI.ENCODEWITHSIGNATURE
abi.encodeWithSelector is much cheaper than abi.encodeWithSignature because it doesn’t require to compute the selector from the string.

File Holographer.sol: [164](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol#L164)

File HolographERC721.sol: [260](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L260)

## [G-12] An array's length should be cached to save gas in for-loops
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.

File PA1D.sol: [432](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432), [437](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L432), [454](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L454), [474](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol#L474)

File HolographERC721.sol: [357](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L357)

File HolographERC20.sol: [564](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L564)

## [G-13] if condition is unesesary 
It does not need from if condition here. With removing of body of if condition will save some deployment gas

File HolographERC721.sol: [1003](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol#L1003)

File HolographERC20.sol: [755](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L755), [288-294](https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol#L288-L294), 


Recommended: `return (_eventConfig >> uint256(_eventName)) & uint256(1) == 1;`
PoC
```
pragma solidity 0.8.7;

contract HolographWorst {

   function sum1(uint256 param) public returns (bool)
  {
    ++param;
    return param == 1 ? true : false;
  }
}
```

```
pragma solidity 0.8.7;

contract HolographBest {

   function sum1(uint256 param) public returns (bool)
  {
    ++param;
    return param == 1;
  }
}
```

HolographWorst deployment: 176669 gas
HolographWorst transaction: 25230 gas
HolographBest deployment: 173219 gas
HolographBest transaction: 25210 gas

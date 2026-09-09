# Chapitre 3 -- Signatures enveloppees : ERC-6492 et ERC-8010

Ces deux modules resolvent un meme probleme sous deux angles differents :
comment verifier une signature quand le compte signataire n a pas encore de
code deploye sur la chaine au moment de la verification.

`erc6492/SignatureErc6492.ts` cible le cas des smart contracts wallets
"counterfactuels" -- une adresse deterministe (par exemple via CREATE2) dont
le contrat n est pas encore deploye. Une signature ERC-6492 enveloppee
encode trois elements : l adresse cible `to` pour la verification
counterfactuelle, la `data` (calldata) a executer sur cette cible pour
declencher son deploiement, et la signature originale. Le module embarque
directement le bytecode compile d un contrat de "validation universelle de
signature" (`universalSignatureValidatorBytecode`, deployable sans etat via
un simple appel de creation), ainsi que son ABI, permettant de verifier la
signature meme si le wallet n existe pas encore sur la chaine au moment de
l appel.

`erc8010/SignatureErc8010.ts` cible un cas plus specifique a l account
abstraction native de type EIP-7702 : une "Authorization" (la structure
EIP-7702 qui delegue le code d un EOA vers un contrat) qui n a pas encore
ete incluse dans une transaction on-chain. La fonction `wrap` prend une
`authorization` signee, une `signature` originale, et une `data`
d initialisation optionnelle, encode cette `authorization` avec les
parametres ABI `suffixParameters`
(`(chainId, delegation, nonce, yParity, r, s), address to, bytes data`), et
l ajoute en suffixe de la signature originale suivie des `magicBytes`
`0x8010801080108010...` qui permettent de reconnaitre une signature ERC-8010
enveloppee. La fonction `unwrap` fait l operation inverse : elle lit la
longueur du suffixe (les 32 octets juste avant les `magicBytes`), en extrait
l `authorization`, la `signature` d origine, et l eventuelle `data`/`to`.

Les deux mecanismes partagent le meme objectif : permettre a un verifieur
de signature de "voir a travers" une signature enveloppee pour retrouver la
preuve cryptographique reelle, meme quand le compte qui a signe n a pas
encore d etat on-chain qui confirme son autorite a signer.

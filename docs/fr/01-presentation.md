# Chapitre 1 -- Presentation de based-ox

Ce depot est le fork/mainteneur Base de "ox" (le nom du paquet npm publie
reste litteralement `ox`), une bibliotheque standard TypeScript pour
Ethereum ecrite a l origine par jxom (wevm). Le `CHANGELOG.md` du paquet
reference directement les pull requests historiques de wevm/ox, confirmant
que ce depot poursuit la meme ligne de developpement plutot que de repartir
de zero. Ox fournit des utilitaires bas niveau, sans etat et types
strictement pour les primitives Ethereum : ABIs, adresses, blocs, bytes,
ECDSA, hexadecimal, JSON-RPC, RLP, signatures, enveloppes de transaction,
etc. Le README le decrit explicitement comme non-opinionated (sans parti
pris) : concu pour etre consomme par des couches de plus haut niveau (Viem,
Tevm) qui lui ajoutent leur propre interface, ou directement quand on a
besoin de primitives bas niveau sans adopter toute une stack de client.

L organisation du code source (`src/`) separe un module `core/` (les
primitives generiques ci-dessus) de modules nommes par standard ERC :
`erc4337/` (comptes abstraits, UserOperation), `erc6492/` (validation de
signature pour les contrats non deployes/counterfactuels), `erc7821/`
(execution d appels groupes) et `erc8010/` (signatures enveloppees pour les
delegations EIP-7702 pas encore activees). Un dossier `contracts/` contient
du Solidity compile via Foundry (notamment un validateur de signature
universel deployable sans etat), et `site/` heberge la documentation
(Vocs) publiee sur oxlib.sh.

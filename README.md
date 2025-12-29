## Vérification des services (health check)
<img width="566" height="177" alt="image" src="https://github.com/user-attachments/assets/a34e15ac-1caa-44c4-a3aa-1d0a06125bb2" />

## Créer un livre avec stock connu
<img width="561" height="98" alt="image" src="https://github.com/user-attachments/assets/c5c70c4b-dc88-43f4-b528-e09950f10707" />

## Récupérer l’ID du livre
<img width="571" height="67" alt="image" src="https://github.com/user-attachments/assets/0c15c457-a5b3-4575-90d4-3258ce5e6eee" />

## Tester borrow une fois (sans concurrence)
<img width="552" height="68" alt="image" src="https://github.com/user-attachments/assets/639dcf8a-0a01-4243-91f5-ae5d9da7970a" />

## Lancer le test loadtest.sh
<img width="566" height="265" alt="image" src="https://github.com/user-attachments/assets/baaba650-6735-4092-ab46-99ca2853735e" />

## Lancer le test loadtest.ps1
<img width="570" height="146" alt="image" src="https://github.com/user-attachments/assets/bdd6f376-de6a-4540-8d35-e921d38c49b9" />

## Lire l’état du stock final
<img width="563" height="82" alt="image" src="https://github.com/user-attachments/assets/c5d16e99-052a-4be8-83a6-230d8cfd0231" />

## Stop pricing-service
<img width="638" height="278" alt="image" src="https://github.com/user-attachments/assets/1a4de24c-2771-47bd-81a5-165c47944c39" />

## Créer un nouveau livre avec stock 10
<img width="598" height="72" alt="image" src="https://github.com/user-attachments/assets/5d522265-c1ba-475f-815d-43ed20fd5670" />

## Récupérer l’ID fallback :
<img width="604" height="57" alt="image" src="https://github.com/user-attachments/assets/14c7d0e7-cd75-4997-bbf1-d6d3c5199b59" />

## Relancer le test de charge (30 requêtes)
<img width="607" height="184" alt="image" src="https://github.com/user-attachments/assets/8e832c4b-34d8-43d1-a473-ef1fbce839d2" />

## Conclusion
Le verrou DB pessimiste est essentiel en multi-instances car sans lui, deux instances peuvent lire simultanément le même stock, le décrémenter en parallèle et créer des incohérences (stock négatif ou pertes d'emprunts). Nos tests ont prouvé qu'avec le verrou, malgré 50 requêtes concurrentes sur 3 instances, exactement 10 emprunts ont été autorisés avec un stock final à 0.
Le Circuit Breaker détecte les services défaillants et arrête temporairement les appels pour éviter la surcharge et la propagation d'erreurs. Le fallback fournit une réponse dégradée (price=0.0) quand pricing-service est down, permettant à book-service de rester fonctionnel. Cette combinaison garantit la résilience : lors de nos tests avec pricing arrêté, les 10 emprunts ont réussi avec le fallback, prouvant que le système reste opérationnel même en mode dégradé.




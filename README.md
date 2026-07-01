# kd_together

Description Globale du Projet

Ce projet gère le processus de commande dans un système e-commerce ou de gestion d'inventaire. Il s'articule autour de plusieurs services qui collaborent pour assurer la prise en charge, le traitement, la validation et la livraison des commandes. Les principaux services sont :

    Order Service : responsable de la création et de la gestion des commandes.
    Payment Service : gère le paiement, vérifie la transaction et notifie la réussite ou l’échec du paiement.
    Stock Service : contrôle la disponibilité des produits, met à jour le stock lors de la commande ou du retour.
    Delivery Service : organise la livraison, confirme la livraison et met à jour le statut de la commande.

Le flux d’événements suit une séquence précise, depuis la création de la commande jusqu’à la livraison, avec des événements clés pour chaque étape.
Rôles de chaque service
Service 	Rôle principal
Order Service 	Crée la commande, déclenche le processus, et met à jour le statut de la commande.
Payment Service 	Gère le paiement, vérifie la transaction, et notifie le succès ou l’échec du paiement.
Stock Service 	Vérifie la disponibilité des produits, met à jour le stock lors de la commande ou du retour.
Delivery Service 	Organise la livraison, confirme la livraison, et met à jour le statut final de la commande.


# Events/Choreography

## 1. Order Service
- Saves a new order, sets the state as **ORDER_CREATED_EVENT**.
- Publie un événement appelé **ORDER_CREATED_EVENT**.

## 2. Payment Service
- Écoute **ORDER_CREATED_EVENT**.
- Charge le client et publie l'événement **BILLED_ORDER_EVENT**.

## 3. Stock Service
- Écoute **BILLED_ORDER_EVENT**.
- Met à jour le stock, prépare les produits achetés dans la commande, et publie **ORDER_PREPARED_EVENT**.

## 4. Delivery Service
- Écoute **ORDER_PREPARED_EVENT**.
- Prend en charge la livraison du produit.
- À la fin, publie **ORDER_DELIVERED_EVENT**.

## 5. Order Service
- Écoute **ORDER_DELIVERED_EVENT**.
- Met à jour l’état de la commande.

---

# Rolling back in Saga’s Events/Choreography

## 1. Stock Service
- Produit **PRODUCT_OUT_OF_STOCK_EVENT**.

## 2. Both Order Service and Payment Service
- Écoutent le message précédent.

## 3. Payment Service
- Rembourse le client.

## 4. Order Service
- Met à jour l’état de la commande en échec.

<img width="1402" height="1122" alt="3927c476-650a-4f08-b3cc-a8c8b26f2ed6" src="https://github.com/user-attachments/assets/d15f3a67-c8c2-4860-ac7f-905e0cc3f651" />


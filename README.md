
import pygame
import sys

pygame.init()

# Paramètres du jeu
largeur, hauteur = 800, 600
fenetre = pygame.display.set_mode((largeur, hauteur))
pygame.display.set_caption("Jeu de Dinosaures")

# Couleurs
blanc = (255, 255, 255)
noir = (0, 0, 0)

# Dinosaure
dinosaure_x, dinosaure_y = 50, hauteur - 50
dinosaure_largeur, dinosaure_hauteur = 50, 50
dinosaure_vitesse = 5
saut = False
saut_compteur = 10

# Obstacle
obstacle_largeur, obstacle_hauteur = 30, 30
obstacle_x, obstacle_y = largeur, hauteur - obstacle_hauteur
obstacle_vitesse = 5

score = 0

# Police pour afficher le score
police = pygame.font.Font(None, 36)

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

        # Gestion des sauts
        if event.type == pygame.KEYDOWN and event.key == pygame.K_SPACE and not saut:
            saut = True

    # Logique du jeu
    keys = pygame.key.get_pressed()
    if keys[pygame.K_RIGHT]:
        dinosaure_x += dinosaure_vitesse
    if keys[pygame.K_LEFT]:
        dinosaure_x -= dinosaure_vitesse

    # Limiter le dinosaure à l'intérieur de l'écran
    dinosaure_x = max(0, min(dinosaure_x, largeur - dinosaure_largeur))

    # Gestion du saut
    if saut:
        if saut_compteur >= -10:
            neg = 1
            if saut_compteur < 0:
                neg = -1
            dinosaure_y -= (saut_compteur ** 2) * 0.5 * neg
            saut_compteur -= 1
        else:
            saut = False
            saut_compteur = 10

    # Mouvement de l'obstacle
    obstacle_x -= obstacle_vitesse

    # Réinitialiser l'obstacle s'il sort de l'écran
    if obstacle_x < 0:
        obstacle_x = largeur
        obstacle_y = hauteur - obstacle_hauteur
        score += 1  # Augmenter le score lorsque l'obstacle est évité

    # Vérifier la collision avec l'obstacle
    if (
        dinosaure_x < obstacle_x + obstacle_largeur
        and dinosaure_x + dinosaure_largeur > obstacle_x
        and dinosaure_y < obstacle_y + obstacle_hauteur
        and dinosaure_y + dinosaure_hauteur > obstacle_y
    ):
        # Gérer la collision ici, par exemple, afficher un message ou redémarrer le jeu.
        print("Collision ! Score final :", score)
        pygame.quit()
        sys.exit()

    # Dessiner l'écran
    fenetre.fill(blanc)

    # Dessiner le dinosaure
    pygame.draw.rect(fenetre, noir, (dinosaure_x, dinosaure_y, dinosaure_largeur, dinosaure_hauteur))

    # Dessiner l'obstacle
    pygame.draw.rect(fenetre, noir, (obstacle_x, obstacle_y, obstacle_largeur, obstacle_hauteur))

    # Afficher le score
    texte_score = police.render("Score : " + str(score), True, noir)
    fenetre.blit(texte_score, (10, 10))

    # Mise à jour de l'affichage
    pygame.display.flip()

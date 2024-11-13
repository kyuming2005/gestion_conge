
# Création d'une page statistiques des congés

La page de statistiques permet d'afficher des informations agrégées sur les congés, comme le nombre total de congés pris, la répartition par type de congé, etc.

## 1. Création du StatsController

Commencez par générer un nouveau contrôleur pour la page des statistiques :

```bash
php bin/console make:controller StatsController
```

Cela créera un fichier `StatsController.php` dans `src/Controller` et un fichier de vue associé `stats/index.html.twig` dans `templates/stats`.

## 2. Ajouter des méthodes dans le StatsController

Dans le contrôleur `StatsController`, vous pouvez définir des méthodes pour obtenir les données de la base de données. Voici quelques exemples :

### Méthode utilisant le Repository (sans SQL)

Le Repository permet d'utiliser des méthodes de Symfony sans écrire de SQL.

Dans `StatsController`, injectez le repository de `Conge` pour accéder aux données de cette entité :

```php
use App\Repository\CongeRepository;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class StatsController extends AbstractController
{
    #[Route('/stats', name: 'app_stats')]
    public function index(CongeRepository $congeRepository): Response
    {
        // Exemples de données statistiques
        $totalConges = $congeRepository->count([]);
        $congesParType = $congeRepository->createQueryBuilder('c')
            ->select('c.type, COUNT(c.id) as count')
            ->groupBy('c.type')
            ->getQuery()
            ->getResult();

        return $this->render('stats/index.html.twig', [
            'totalConges' => $totalConges,
            'congesParType' => $congesParType,
        ]);
    }
}
```

- `$totalConges` : Compte le nombre total de congés dans la base de données.
- `$congesParType` : Agrège les congés par type, par exemple "Vacances", "Maladie", etc.

### Méthode avec SQL Direct

Si vous souhaitez utiliser SQL directement, vous pouvez accéder à l'Entity Manager pour exécuter des requêtes SQL brutes.

Ajoutez cette méthode dans `StatsController` :

```php
use Doctrine\ORM\EntityManagerInterface;

#[Route('/stats/sql', name: 'app_stats_sql')]
public function indexWithSQL(EntityManagerInterface $entityManager): Response
{
    // Requête SQL directe pour compter les congés par utilisateur
    $connection = $entityManager->getConnection();
    $sql = 'SELECT user_id, COUNT(id) as count FROM conge GROUP BY user_id';
    $stmt = $connection->prepare($sql);
    $result = $stmt->executeQuery()->fetchAllAssociative();

    return $this->render('stats/index.html.twig', [
        'congesParUser' => $result,
    ]);
}
```

Cette méthode utilise SQL directement pour obtenir le nombre de congés pris par utilisateur.

## 3. Création de la vue des Statistiques

Ensuite, modifiez le fichier `templates/stats/index.html.twig` pour afficher les données :

```twig
{% extends 'base.html.twig' %}

{% block title %}Statistiques{% endblock %}

{% block body %}
<h1>Statistiques des Congés</h1>

<h2>Total des congés</h2>
<p>Nombre total de congés : {{ totalConges }}</p>

<h2>Répartition des congés par Type</h2>
<table class="table">
    <thead>
        <tr>
            <th>Type de Congé</th>
            <th>Nombre</th>
        </tr>
    </thead>
    <tbody>
    {% for conge in congesParType %}
        <tr>
            <td>{{ conge.type }}</td>
            <td>{{ conge.count }}</td>
        </tr>
    {% else %}
        <tr>
            <td colspan="2">Aucun congé trouvé</td>
        </tr>
    {% endfor %}
    </tbody>
</table>

<h2>Nombre de congés par utilisateur (via SQL direct)</h2>
<table class="table">
    <thead>
        <tr>
            <th>Utilisateur ID</th>
            <th>Nombre de Congés</th>
        </tr>
    </thead>
    <tbody>
    {% for data in congesParUser %}
        <tr>
            <td>{{ data.user_id }}</td>
            <td>{{ data.count }}</td>
        </tr>
    {% else %}
        <tr>
            <td colspan="2">Aucune donnée trouvée</td>
        </tr>
    {% endfor %}
    </tbody>
</table>

{% endblock %}
```

## 4. Accéder à la page de statistiques

Enfin, accédez à la page des statistiques en lançant votre serveur Symfony et en vous rendant sur l'URL suivante :

```bash
http://localhost:8000/stats
```
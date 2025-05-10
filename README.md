using UnityEngine;

public class TerrainGenerator : MonoBehaviour
{
    public Terrain terrain;
    public float heightMultiplier = 10f;
    public float detailScale = 0.1f;

    void Start()
    {
        GenerateTerrain();
    }

    void GenerateTerrain()
    {
        TerrainData terrainData = terrain.terrainData;
        int width = terrainData.heightmapResolution;
        int height = terrainData.heightmapResolution;
        float[,] heights = new float[width, height];

        for (int x = 0; x < width; x++)
        {
            for (int y = 0; y < height; y++)
            {
                heights[x, y] = Mathf.PerlinNoise(x * detailScale, y * detailScale) * heightMultiplier;
            }
        }

        terrainData.SetHeights(0, 0, heights);
    }
}using UnityEngine;

public class CurvedRoadGenerator : MonoBehaviour
{
    public GameObject roadPrefab;
    public Transform player;
    private float roadLength = 20f;
    private float spawnZ = 0f;
    private int roadCount = 5;
    private float curveFactor = 5f;

    void Start()
    {
        for (int i = 0; i < roadCount; i++)
        {
            SpawnRoad();
        }
    }

    void Update()
    {
        if (player.position.z > spawnZ - (roadCount * roadLength))
        {
            SpawnRoad();
        }
    }

    void SpawnRoad()
    {
        float curveOffset = Mathf.Sin(spawnZ / curveFactor) * curveFactor;
        Vector3 spawnPosition = new Vector3(curveOffset, 0, spawnZ);
        GameObject road = Instantiate(roadPrefab, spawnPosition, Quaternion.identity);
        spawnZ += roadLength;
    }
}using UnityEngine;

public class EndlessRoadGenerator : MonoBehaviour
{
    public GameObject roadPrefab;
    public Transform player;
    private float roadLength = 20f;
    private float spawnZ = 0f;
    private int roadCount = 5;

    void Start()
    {
        for (int i = 0; i < roadCount; i++)
        {
            SpawnRoad();
        }
    }

    void Update()
    {
        if (player.position.z > spawnZ - (roadCount * roadLength))
        {
            SpawnRoad();
        }
    }

    void SpawnRoad()
    {
        GameObject road = Instantiate(roadPrefab, new Vector3(0, 0, spawnZ), Quaternion.identity);
        spawnZ += roadLength;
    }
}

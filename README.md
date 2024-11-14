using UnityEngine;
using System.Collections.Generic;

public class FishAI : MonoBehaviour
{
    public float swimSpeed = 2f;
    public float evadeSpeed = 4f;
    public float detectionRadius = 5f;
    public Transform player;

    private Vector3 targetPosition;
    private bool isEvading = false;

    void Start()
    {
        SetRandomTargetPosition();
    }

    void Update()
    {
        float distanceToPlayer = Vector3.Distance(transform.position, player.position);

        if (distanceToPlayer < detectionRadius && !isEvading)
        {
            StartEvading();
        }
        else if (isEvading && distanceToPlayer >= detectionRadius)
        {
            StopEvading();
        }

        MoveToTarget();
    }

    private void SetRandomTargetPosition()
    {
        targetPosition = new Vector3(Random.Range(-10, 10), transform.position.y, Random.Range(-10, 10));
    }

    private void MoveToTarget()
    {
        float step = (isEvading ? evadeSpeed : swimSpeed) * Time.deltaTime;
        transform.position = Vector3.MoveTowards(transform.position, targetPosition, step);

        if (Vector3.Distance(transform.position, targetPosition) < 1f)
        {
            SetRandomTargetPosition();
        }
    }

    private void StartEvading()
    {
        isEvading = true;
        targetPosition = (transform.position - player.position).normalized * evadeSpeed;
    }

    private void StopEvading()
    {
        isEvading = false;
        SetRandomTargetPosition();
    }
}


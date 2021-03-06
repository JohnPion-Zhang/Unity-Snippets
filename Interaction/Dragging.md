# Dragging

## Drag GameObject From Center

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour {

    private Vector3 GetMousePosition() {

        return Camera.main.ScreenToWorldPoint(new Vector3(
            Input.mousePosition.x,
            Input.mousePosition.y,
            Camera.main.WorldToScreenPoint(gameObject.transform.position).z
        ));

    }

    void OnMouseDrag() {

        gameObject.transform.position = GetMousePosition();

    }

}
```

## Drag GameObject From Point

```csharp
using UnityEngine;

public class SampleController : MonoBehaviour
{

    public LayerMask layerMask = ~0;

    private Camera mainCamera;

    private Transform dragObjectTransform;
    private float dragDistance = 0;
    private Vector3 dragOffset = Vector3.zero;

    private void Awake()
    {

        mainCamera = Camera.main;

    }

    private void Update()
    {

        if (Input.GetMouseButtonDown(0))
        {

            RaycastHit hit;

            if (Physics.Raycast(mainCamera.ScreenPointToRay(Input.mousePosition), out hit, Mathf.Infinity, layerMask))
            {

                if (hit.transform.gameObject == gameObject)
                {

                    dragObjectTransform = hit.transform;
                    dragDistance = hit.distance;
                    dragOffset = dragObjectTransform.transform.position - hit.point;

                }

            }

        }

        if (dragObjectTransform != null)
        {

            if (Input.GetMouseButton(0))
            {

                dragObjectTransform.position = mainCamera.ScreenPointToRay(Input.mousePosition).GetPoint(dragDistance) + dragOffset;

            }
            else if (Input.GetMouseButtonUp(0))
            {

                dragObjectTransform = null;
                dragDistance = 0;
                dragOffset = Vector3.zero;

            }

        }

    }

}
```

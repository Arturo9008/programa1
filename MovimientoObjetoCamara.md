//Codigo movimiento de un Onjeto en 3D en Csharp con Unity3D
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float horizontalMove;
    public float verticalMove;
    private Vector3 playerInput;

    public CharacterController player;

    //Variable para ajustar la velocidad
    public float playerSpeed = 16;
    private Vector3 movePlayer;

    public Camera mainCamera;
    private Vector3 camForward;
    private Vector3 camRight;

    // Start is called before the first frame update
    void Start()
    {
        player = GetComponent<CharacterController>();
    }

    // Update is called once per frame
    void Update()
    {
        horizontalMove = Input.GetAxis("Horizontal");
        verticalMove = Input.GetAxis("Vertical");

        playerInput = new Vector3(horizontalMove, 0, verticalMove);
        playerInput = Vector3.ClampMagnitude(playerInput, 1);

        camDirection();

        movePlayer = playerInput.x * camRight + playerInput.z * camForward;

        //Hacia donde Mira el Player(Jugador)
        player.transform.LookAt(player.transform.position + movePlayer);

        player.Move(movePlayer * playerSpeed * Time.deltaTime);
    }

    //Funcion de la camara
    void camDirection()
    {
        camForward = mainCamera.transform.forward;
        camRight = mainCamera.transform.right;

        camForward.y = 0;
        camRight.y = 0;

        //Valor normalizado de la direccion
        camForward = camForward.normalized;
        camRight = camRight.normalized;
    }
}

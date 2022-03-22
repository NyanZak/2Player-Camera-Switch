2 Player Camera Switch Guide
======================

This documentation describes how to use the `2 Player Camera Switch` component in your project.

Behaviours
----------

-   \[`CharacterSwap`\]
-   \[`Collectable`\]
-   \[`ScoreManager`\]
-   \[`WallDestroy`\]

CharacterSwap
------------

Description

### Properties

-   `Character` 
-   `Possible Characters` 
-   `Which Character`
-   `Cam` 

### Script

using Cinemachine;

    public Transform character;
    public List<Transform> possibleCharacters;
    public int whichCharacter;
    public CinemachineVirtualCamera cam;
    
    
   void Start()
    {
        if (character == null && possibleCharacters.Count >= 1)
        {
            character = possibleCharacters[0];
        }
        Swap();
    }
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Q))
        {
            if (whichCharacter == 0)
            {
                whichCharacter = possibleCharacters.Count - 1;
            }
            else
            {
                whichCharacter -= 1;
            }
            Swap();
        }
        if (Input.GetKeyDown(KeyCode.E))
        {
            if (whichCharacter == possibleCharacters.Count - 1)
            {
                whichCharacter = 0;
            }
            else
            {
                whichCharacter += 1;
            }
            Swap();
        }
    }
    
    
    public void Swap()
    {
        character = possibleCharacters[whichCharacter];
        character.GetComponent<movementScript>().enabled = true;
        character.GetComponent<Renderer>().material.color = Color.green;
        for (int i = 0; i < possibleCharacters.Count; i++)
        {
            if (possibleCharacters[i] != character)
            {
                possibleCharacters[i].GetComponent<movementScript>().enabled = false;
                possibleCharacters[i].GetComponent<Renderer>().material.color = Color.red;

            }
        }
        cam.LookAt = character;
        cam.Follow = character;
    }
}

Collectable
--------

Description

### Script

private void OnTriggerEnter(Collider other)
    {
        if (other.tag == "Player1")
        {
            scoreManager.instance.AddPlayer1Score();
            Destroy(gameObject);
        }
        if (other.tag == "Player2")
        {
            scoreManager.instance.AddPlayer2Score();
            Destroy(gameObject);
        }
    }
}

scoreManager
------------

Description

### Properties

-   `Player1ScoreText`
-   `Player2ScoreText` 

### Script

using UnityEngine.UI;
using TMPro;

    public static scoreManager instance;
    public static int player1score, player2score;
    public TMP_Text player1scoreText, player2scoreText;
    
    
    private void Awake()
    {
        instance = this;
    }
    public void AddPlayer1Score()
    {
        player1score++;
        player1scoreText.text = player1score.ToString();
    }
    public void AddPlayer2Score()
    {
        player2score++;
        player2scoreText.text = player2score.ToString();
    }
}

wallDestroy
----------

Description
### Properties

-   `Wall` -

### Script

    public GameObject wall;
    private void OnTriggerEnter(Collider other)
    {
        Destroy(wall);
    }
}

```swarmauri/core/README.md

# Core Library

The Core Library provides the foundational interfaces and abstract base classes necessary for developing scalable and flexible machine learning agents, models, and tools. It is designed to offer a standardized approach to implementing various components of machine learning systems, such as models, parsers, conversations, and vector stores.

## Features

- **Models Interface**: Define and interact with predictive models.
- **Agents Interface**: Build and manage intelligent agents for varied tasks.
- **Tools Interface**: Develop tools with standardized execution and configuration.
- **Parsers and Conversations**: Handle and parse text data, manage conversations states.
- **Vector Stores**: Interface for vector storage and similarity searches.
- **Document Stores**: Manage the storage and retrieval of documents.
- **Retrievers and Chunkers**: Efficiently retrieve relevant documents and chunk large text data.

## Getting Started

To start developing with the Core Library, include it as a module in your Python project. Ensure you have Python 3.6 or later installed.

```python
# Example of using an abstract model interface from the Core Library
from swarmauri.core.models.IModel import IModel

class MyModel(IModel):
    # Implement the abstract methods here
    pass
```

## Documentation

For more detailed documentation on each interface and available abstract classes, refer to the [Docs](/docs) directory within the library.

## Contributing

Contributions are welcome! If you'd like to add a new feature, fix a bug, or improve documentation, please submit a pull request.

## License

See `LICENSE` for more information.


```

```swarmauri/core/__init__.py



```

```swarmauri/core/models/__init__.py



```

```swarmauri/core/models/IPredict.py

from abc import ABC, abstractmethod

class IPredict(ABC):
    """
    Interface for making predictions with models.
    """

    @abstractmethod
    def predict(self, input_data) -> any:
        """
        Generate predictions based on the input data provided to the model.
        """
        pass

```

```swarmauri/core/models/IFit.py

from abc import ABC, abstractmethod

class IFit(ABC):
    """
    Interface for training models.
    """

    @abstractmethod
    def fit(self, X_train, y_train, epochs: int, batch_size: int) -> None:
        """
        Train the model on the provided dataset.
        """
        pass

```

```swarmauri/core/models/IModel.py

from abc import ABC, abstractmethod

class IModel(ABC):
    """
    Interface focusing on the basic properties and settings essential for defining models.
    """

    @property
    @abstractmethod
    def model_name(self) -> str:
        """
        Get the name of the model.
        """
        pass

    @model_name.setter
    @abstractmethod
    def model_name(self, value: str) -> None:
        """
        Set the name of the model.
        """
        pass

```

```swarmauri/core/agent_apis/__init__.py



```

```swarmauri/core/agent_apis/IAgentCommands.py

from abc import ABC, abstractmethod
from typing import Callable, Any, List

class IAgentCommands(ABC):
    """
    Interface for the API object that enables a SwarmAgent to host various API routes.
    """


    @abstractmethod
    def invoke(self, request: Any) -> Any:
        """
        Handles invocation requests synchronously.
        
        Parameters:
            request (Any): The incoming request payload.

        Returns:
            Any: The response payload.
        """
        pass

    @abstractmethod
    async def ainvoke(self, request: Any) -> Any:
        """
        Handles invocation requests asynchronously.
        
        Parameters:
            request (Any): The incoming request payload.

        Returns:
            Any: The response payload.
        """
        pass

    @abstractmethod
    def batch(self, requests: List[Any]) -> List[Any]:
        """
        Handles batched invocation requests synchronously.
        
        Parameters:
            requests (List[Any]): A list of incoming request payloads.

        Returns:
            List[Any]: A list of responses.
        """
        pass

    @abstractmethod
    async def abatch(self, requests: List[Any]) -> List[Any]:
        """
        Handles batched invocation requests asynchronously.
        
        Parameters:
            requests (List[Any]): A list of incoming request payloads.

        Returns:
            List[Any]: A list of responses.
        """
        pass

    @abstractmethod
    def stream(self, request: Any) -> Any:
        """
        Handles streaming requests.
        
        Parameters:
            request (Any): The incoming request payload.
        
        Returns:
            Any: A streaming response.
        """
        pass

    @abstractmethod
    def get_schema_config(self) -> dict:
        """
        Retrieves the schema configuration for the API.

        Returns:
            dict: The schema configuration.
        """
        pass

```

```swarmauri/core/agent_apis/IAgentRouterCRUD.py

from abc import ABC, abstractmethod
from typing import Callable, Any, Dict

class IAgentRouterCRUD(ABC):
    """
    Interface for managing API routes within a SwarmAgent.
    """
    
    @abstractmethod
    def create_route(self, path: str, method: str, handler: Callable[[Any], Any]) -> None:
        """
        Create a new route for the API.
        
        Parameters:
        - path (str): The URL path for the route.
        - method (str): The HTTP method (e.g., 'GET', 'POST').
        - handler (Callable[[Any], Any]): The function that handles requests to this route.
        """
        pass
    
    @abstractmethod
    def read_route(self, path: str, method: str) -> Dict:
        """
        Retrieve information about a specific route.
        
        Parameters:
        - path (str): The URL path for the route.
        - method (str): The HTTP method.
        
        Returns:
        - Dict: Information about the route, including path, method, and handler.
        """
        pass
    
    @abstractmethod
    def update_route(self, path: str, method: str, new_handler: Callable[[Any], Any]) -> None:
        """
        Update the handler function for an existing route.
        
        Parameters:
        - path (str): The URL path for the route.
        - method (str): The HTTP method.
        - new_handler (Callable[[Any], Any]): The new function that handles requests to this route.
        """
        pass
    
    @abstractmethod
    def delete_route(self, path: str, method: str) -> None:
        """
        Delete a specific route from the API.
        
        Parameters:
        - path (str): The URL path for the route.
        - method (str): The HTTP method.
        """
        pass

```

```swarmauri/core/conversations/__init__.py



```

```swarmauri/core/conversations/IMaxSize.py

from abc import ABC, abstractmethod

class IMaxSize(ABC):

    @property
    @abstractmethod
    def max_size(self) -> int:
        """
        """
        pass

    @max_size.setter
    @abstractmethod
    def max_size(self, new_max_size: int) -> None:
        """ 
        """
        pass

```

```swarmauri/core/conversations/IConversation.py

from abc import ABC, abstractmethod
from typing import List, Optional
from ..messages.IMessage import IMessage

class IConversation(ABC):
    """
    Interface for managing conversations, defining abstract methods for
    adding messages, retrieving the latest message, getting all messages, and clearing history.
    """

    @property
    def history(self) -> List[IMessage]:
        """
        Provides read-only access to the conversation history.
        """
        pass

    @abstractmethod
    def add_message(self, message: IMessage):
        """
        Adds a message to the conversation history.
        """
        pass

    @abstractmethod
    def get_last(self) -> Optional[IMessage]:
        """
        Retrieves the latest message from the conversation history.
        """
        pass

    @abstractmethod
    def clear_history(self) -> None:
        """
        Clears the conversation history.
        """
        pass

    @abstractmethod
    def as_dict(self) -> List[dict]:
        """
        Returns all messages from the conversation history as a list of dictionaries.
        """
        pass

```

```swarmauri/core/conversations/ISystemContext.py

from abc import ABC, abstractmethod
from typing import Optional
from ..messages.IMessage import IMessage

class ISystemContext(ABC):

    @property
    @abstractmethod
    def system_context(self) -> Optional[IMessage]:
        """
        An abstract property to get the system context message.
        Subclasses must provide an implementation for storing and retrieving system context.
        """
        pass

    @system_context.setter
    @abstractmethod
    def system_context(self, new_system_message: Optional[IMessage]) -> None:
        """
        An abstract property setter to update the system context.
        Subclasses must provide an implementation for how the system context is updated.
        This might be a direct string, which is converted to an IMessage instance, or directly an IMessage instance.
        """
        pass

```

```swarmauri/core/documents/__init__.py



```

```swarmauri/core/documents/IDocument.py

from abc import ABC, abstractmethod
from typing import Dict

class IDocument(ABC):
    @abstractmethod
    def __init__(self, doc_id, content, metadata: Dict):
        pass

    @property
    @abstractmethod
    def id(self) -> str:
        """
        Get the document's ID.
        """
        pass

    @id.setter
    @abstractmethod
    def id(self, value: str) -> None:
        """
        Set the document's ID.
        """
        pass

    @property
    @abstractmethod
    def content(self) -> str:
        """
        Get the document's content.
        """
        pass

    @content.setter
    @abstractmethod
    def content(self, value: str) -> None:
        """
        Set the document's content.
        """
        pass

    @property
    @abstractmethod
    def metadata(self) -> Dict:
        """
        Get the document's metadata.
        """
        pass

    @metadata.setter
    @abstractmethod
    def metadata(self, value: Dict) -> None:
        """
        Set the document's metadata.
        """
        pass

    # Including the abstract methods __str__ and __repr__ definitions for completeness.
    @abstractmethod
    def __str__(self) -> str:
        pass

    @abstractmethod
    def __repr__(self) -> str:
        pass
    
    def __setitem__(self, key, value):
        """Allow setting items like a dict for metadata."""
        self.metadata[key] = value

    def __getitem__(self, key):
        """Allow getting items like a dict for metadata."""
        return self.metadata.get(key)

```

```swarmauri/core/documents/IEmbed.py

from abc import ABC, abstractmethod
from typing import Dict
from swarmauri.core.vectors.IVector import IVector

class IEmbed(ABC):
    @property
    @abstractmethod
    def embedding(self) -> IVector:
        """
        Get the document's embedding.
        """
        pass

    @embedding.setter
    @abstractmethod
    def embedding(self, value: IVector) -> None:
        """
        Set the document's embedding.
        """
        pass



```

```swarmauri/core/messages/IMessage.py

from abc import ABC, abstractmethod

class IMessage(ABC):
    """
    An abstract interface representing a general message structure.

    This interface defines the basic attributes that all
    messages should have, including type, name, and content, 
    and requires subclasses to implement representation and formatting methods.
    """
    @property
    @abstractmethod
    def role(self) -> str:
        pass
    
    @property
    @abstractmethod
    def content(self) -> str:
        pass

    @abstractmethod
    def as_dict(self) -> dict:
        """
        An abstract method that subclasses must implement to return a dictionary representation of the object.
        """
        pass

```

```swarmauri/core/messages/__init__.py

from .IMessage import IMessage

```

```swarmauri/core/parsers/__init__.py



```

```swarmauri/core/parsers/IParser.py

from abc import ABC, abstractmethod
from typing import List, Union, Any
from ..documents.IDocument import IDocument

class IParser(ABC):
    """
    Abstract base class for parsers. It defines a public method to parse input data (str or Message) into documents,
    and relies on subclasses to implement the specific parsing logic through protected and private methods.
    """

    @abstractmethod
    def parse(self, data: Union[str, Any]) -> List[IDocument]:
        """
        Public method to parse input data (either a str or a Message) into a list of Document instances.
        
        This method leverages the abstract _parse_data method which must be
        implemented by subclasses to define specific parsing logic.
        """
        pass



```

```swarmauri/core/prompts/__init__.py



```

```swarmauri/core/prompts/IPrompt.py

from abc import ABC, abstractmethod
from typing import Optional, Any

class IPrompt(ABC):
    """
    A base abstract class representing a prompt system.

    Methods:
        __call__: Abstract method that subclasses must implement to enable the instance to be called directly.
    """

    @abstractmethod
    def __call__(self, prompt: Optional[Any]) -> str:
        """
        Abstract method that subclasses must implement to define the behavior of the prompt when called.

        """
        pass


```

```swarmauri/core/agents/__init__.py



```

```swarmauri/core/agents/IAgentToolkit.py

from abc import ABC, abstractmethod
from swarmauri.core.toolkits.IToolkit import IToolkit


class IAgentToolkit(ABC):


    @property
    @abstractmethod
    def toolkit(self) -> IToolkit:
        pass
    
    @toolkit.setter
    @abstractmethod
    def toolkit(self) -> IToolkit:
        pass
    


```

```swarmauri/core/agents/IAgentConversation.py

from abc import ABC, abstractmethod
from swarmauri.core.conversations.IConversation import IConversation

class IAgentConversation(ABC):
    
    @property
    @abstractmethod
    def conversation(self) -> IConversation:
        """
        The conversation property encapsulates the agent's ongoing dialogue or interaction context.
        """
        pass

    @conversation.setter
    @abstractmethod
    def conversation(self) -> IConversation:
        pass

```

```swarmauri/core/agents/IAgentRetriever.py

from abc import ABC, abstractmethod
from swarmauri.core.retrievers.IRetriever import IRetriever 

class IAgentRetriever(ABC):
    
    @property
    @abstractmethod
    def retriever(self) -> IRetriever:
        pass

    @retriever.setter
    @abstractmethod
    def retriever(self) -> IRetriever:
        pass

```

```swarmauri/core/agents/IAgentDocument.py

from abc import ABC, abstractmethod
from swarmauri.core.documents.IDocument import IDocument

class IAgentDocumentStore(ABC):
    
    @property
    @abstractmethod
    def document_store(self) -> IDocument:
        pass

    @documents.setter
    @abstractmethod
    def document_store(self) -> IDocument:
        pass

```

```swarmauri/core/agents/IAgentParser.py

from abc import ABC, abstractmethod
from swarmauri.core.parsers.IParser import IParser 

class IAgentParser(ABC):
    
    @property
    @abstractmethod
    def parser(self) -> IParser:
        pass

    @parser.setter
    @abstractmethod
    def parser(self) -> IParser:
        pass

```

```swarmauri/core/agents/IAgentName.py

from abc import ABC, abstractmethod

class IAgentName(ABC):
    
    @property
    @abstractmethod
    def name(self) -> str:
        """
        The conversation property encapsulates the agent's ongoing dialogue or interaction context.
        """
        pass

    @name.setter
    @abstractmethod
    def name(self) -> str:
        pass

```

```swarmauri/core/agents/IAgent.py

from abc import ABC, abstractmethod
from typing import Any, Optional
from swarmauri.core.models.IModel import IModel

class IAgent(ABC):

    @abstractmethod
    def exec(self, input_data: Optional[Any]) -> Any:
        """
        Executive method that triggers the agent's action based on the input data.
        """
        pass
    
    @property
    @abstractmethod
    def model(self) -> IModel:
        """
        The model property describes the computational model used by the agent.
        """
        pass
    
    @model.setter
    @abstractmethod
    def model(self) -> IModel:

        pass


```

```swarmauri/core/swarms/__init__.py



```

```swarmauri/core/swarms/ISwarm.py

from abc import ABC, abstractmethod
from typing import Any, List, Dict
from swarmauri.core.agents.ISwarmAgent import ISwarmAgent
from swarmauri.core.chains.ICallableChain import ICallableChain

class ISwarm(ABC):
    """
    Interface for a Swarm, representing a collective of agents capable of performing tasks, executing callable chains, and adaptable configurations.
    """

    @abstractmethod
    def add_agent(self, agent: ISwarmAgent) -> None:
        """
        Adds an agent to the swarm.

        Args:
            agent (ISwarmAgent): The agent to be added.
        """
        pass

    @abstractmethod
    def remove_agent(self, agent_id: str) -> bool:
        """
        Removes an agent from the swarm by ID.

        Args:
            agent_id (str): The unique identifier for the agent.
            
        Returns:
            bool: True if the agent was successfully removed; False otherwise.
        """
        pass

    @abstractmethod
    def execute_callable_chain(self, chain: ICallableChain, context: Dict[str, Any] = {}) -> Any:
        """
        Executes a callable chain within the swarm, optionally in a specific execution context.

        Args:
            chain (ICallableChain): The callable chain to be executed.
            context (Dict[str, Any]): Optional context and metadata for executing the chain.
        
        Returns:
            Any: The result of the chain execution.
        """
        pass

    @abstractmethod
    def get_agent(self, agent_id: str) -> ISwarmAgent:
        """
        Retrieves an agent by its ID from the swarm.

        Args:
            agent_id (str): The unique identifier for the agent.

        Returns:
            ISwarmAgent: The agent associated with the given id.
        """
        pass

    @abstractmethod
    def list_agents(self) -> List[ISwarmAgent]:
        """
        Lists all agents currently in the swarm.

        Returns:
            List[ISwarmAgent]: A list of agents in the swarm.
        """
        pass

    @abstractmethod
    def update_swarm_configuration(self, configuration: Dict[str, Any]) -> None:
        """
        Updates the swarm's configuration.

        Args:
            configuration (Dict[str, Any]): The new configuration settings for the swarm.
        """
        pass

    @abstractmethod
    def get_swarm_status(self) -> Dict[str, Any]:
        """
        Retrieves the current status and health information of the swarm, including the number of agents, active tasks, etc.

        Returns:
            Dict[str, Any]: The current status of the swarm.
        """
        pass

```

```swarmauri/core/swarms/ISwarmComponent.py

from abc import ABC, abstractmethod

class ISwarmComponent(ABC):
    """
    Interface for defining a general component within a swarm system.
    """

    @abstractmethod
    def __init__(self, key: str, name: str):
        """
        Initializes a swarm component with a unique key and name.
        """
        pass

```

```swarmauri/core/swarms/ISwarmConfigurationExporter.py

from abc import ABC, abstractmethod
from typing import Dict
class ISwarmConfigurationExporter(ABC):

    @abstractmethod
    def to_dict(self) -> Dict:
        """
        Serializes the swarm configuration to a dictionary.

        Returns:
            Dict: The serialized configuration as a dictionary.
        """
        pass

    @abstractmethod
    def to_json(self) -> str:
        """
        Serializes the swarm configuration to a JSON string.

        Returns:
            str: The serialized configuration as a JSON string.
        """
        pass

    @abstractmethod
    def to_pickle(self) -> bytes:
        """
        Serializes the swarm configuration to a Pickle byte stream.

        Returns:
            bytes: The serialized configuration as a Pickle byte stream.
        """
        pass

```

```swarmauri/core/swarms/ISwarmFactory.py

from abc import ABC, abstractmethod
from typing import Any, Callable, Dict, List, NamedTuple, Optional, Type, Union
from swarmauri.core.swarms.ISwarm import ISwarm
from swarmauri.core.chains.ICallableChain import ICallableChain 
from swarmauri.core.agents.IAgent import IAgent 

class Step(NamedTuple):
    description: str
    callable: Callable  # Reference to the function to execute
    args: Optional[List[Any]] = None
    kwargs: Optional[Dict[str, Any]] = None

class CallableChainItem(NamedTuple):
    key: str  # Unique identifier for the item within the chain
    execution_context: Dict[str, Any]  # Execution context and metadata
    steps: List[Step]

class AgentDefinition(NamedTuple):
    type: str
    configuration: Dict[str, Any]
    capabilities: List[str]
    dependencies: List[str]
    execution_context: Dict[str, Any]

class FunctionParameter(NamedTuple):
    name: str
    type: Type
    default: Optional[Any] = None
    required: bool = True

class FunctionDefinition(NamedTuple):
    identifier: str
    parameters: List[FunctionParameter]
    return_type: Type
    execution_context: Dict[str, Any]
    callable_source: Callable
    
class ISwarmFactory(ABC):

    @abstractmethod
    def create_swarm(self, *args, **kwargs) -> ISwarm:
        """
        Creates and returns a new swarm instance configured with the provided arguments.
        """
        pass

    @abstractmethod
    def create_agent(self, agent_definition: AgentDefinition) -> IAgent:
        """
        Creates a new agent based on the provided enhanced agent definition.
        
        Args:
            agent_definition: An instance of AgentDefinition detailing the agent's setup.
        
        Returns:
            An instance or identifier of the newly created agent.
        """
        pass
    
    @abstractmethod
    def create_callable_chain(self, chain_definition: List[CallableChainItem]) -> ICallableChain:
        """
        Creates a new callable chain based on the provided definition.

        Args:
            chain_definition: Details required to build the chain, such as sequence of functions and arguments.

        Returns:
            ICallableChain: The constructed callable chain instance.
        """
        pass
    
    @abstractmethod
    def register_function(self, function_definition: FunctionDefinition) -> None:
        """
        Registers a function within the factory ecosystem, making it available for callable chains and agents.

        Args:
            function_definition: An instance of FunctionDefinition detailing the function's specification.
        """
        pass

    @abstractmethod
    def export_callable_chains(self, format_type: str = 'json') -> Union[dict, str, bytes]:
        """
        Exports configurations of all callable chains in the specified format.
        Supported formats: 'json', 'pickle'.

        Args:
            format_type (str): The format for exporting the configurations.

        Returns:
            Union[dict, str, bytes]: The callable chain configurations in the specified format.
        """
        pass

    @abstractmethod
    def load_callable_chains(self, chains_data, format_type: str = 'json'):
        """
        Loads callable chain configurations from given data.

        Args:
            chains_data (Union[dict, str, bytes]): Data containing callable chain configurations.
            format_type (str): The format of the provided chains data.
        """
        pass

    @abstractmethod
    def export_configuration(self, format_type: str = 'json') -> Union[dict, str, bytes]:
        """
        Exports the swarm's and agents' configurations in the specified format.
        Supported formats: 'json', 'pickle'. Default is 'json'.

        Args:
            format_type (str): The format for exporting the configurations.

        Returns:
            Union[dict, str, bytes]: The configurations in the specified format.
        """
        pass


```

```swarmauri/core/toolkits/__init__.py



```

```swarmauri/core/toolkits/IToolkit.py

from typing import Dict
from abc import ABC, abstractmethod
from ..tools.ITool import ITool  # Ensure Tool is correctly imported from your tools package

class IToolkit(ABC):
    """
    A class representing a toolkit used by Swarm Agents.
    Tools are maintained in a dictionary keyed by the tool's name.
    """

    @property
    @abstractmethod
    def tools(self) -> Dict[str, ITool]:
        """
        An abstract property that should be implemented by subclasses to return the tools dictionary
        """
        pass

    @abstractmethod
    def add_tools(self, tools: Dict[str, ITool]):
        """
        An abstract method that should be implemented by subclasses to add multiple tools to the toolkit.
        """
        pass

    @abstractmethod
    def add_tool(self, tool: ITool):
        """
        An abstract method that should be implemented by subclasses to add a single tool to the toolkit.
        """
        pass

    @abstractmethod
    def remove_tool(self, tool_name: str):
        """
        An abstract method that should be implemented by subclasses to remove a tool from the toolkit by name.
        """
        pass

    @abstractmethod
    def get_tool_by_name(self, tool_name: str) -> ITool:
        """
        An abstract method that should be implemented by subclasses to retrieve a tool from the toolkit by name.
        """
        pass

    @abstractmethod
    def __len__(self) -> int:
        """
        An abstract method that should be implemented by subclasses to return the number of tools in the toolkit.
        """
        pass

```

```swarmauri/core/tools/__init__.py



```

```swarmauri/core/tools/ITool.py

from abc import ABC, abstractmethod

class ITool(ABC):
    
    @property
    @abstractmethod
    def name(self):
        pass
    
    @property
    @abstractmethod
    def description(self):
        pass
    
    @property
    @abstractmethod
    def parameters(self):
        pass
    
    @abstractmethod
    def as_dict(self):
        pass

    @abstractmethod
    def to_json(obj):
        pass

    @abstractmethod
    def __call__(self, *args, **kwargs):
        pass







```

```swarmauri/core/tools/IParameter.py

from abc import ABC, abstractmethod
from typing import Optional, List, Any

class IParameter(ABC):
    """
    An abstract class to represent a parameter for a tool.
    """

    @property
    @abstractmethod
    def name(self) -> str:
        """
        Abstract property for getting the name of the parameter.
        """
        pass

    @name.setter
    @abstractmethod
    def name(self, value: str):
        """
        Abstract setter for setting the name of the parameter.
        """
        pass

    @property
    @abstractmethod
    def type(self) -> str:
        """
        Abstract property for getting the type of the parameter.
        """
        pass

    @type.setter
    @abstractmethod
    def type(self, value: str):
        """
        Abstract setter for setting the type of the parameter.
        """
        pass

    @property
    @abstractmethod
    def description(self) -> str:
        """
        Abstract property for getting the description of the parameter.
        """
        pass

    @description.setter
    @abstractmethod
    def description(self, value: str):
        """
        Abstract setter for setting the description of the parameter.
        """
        pass

    @property
    @abstractmethod
    def required(self) -> bool:
        """
        Abstract property for getting the required status of the parameter.
        """
        pass

    @required.setter
    @abstractmethod
    def required(self, value: bool):
        """
        Abstract setter for setting the required status of the parameter.
        """
        pass

    @property
    @abstractmethod
    def enum(self) -> Optional[List[Any]]:
        """
        Abstract property for getting the enum list of the parameter.
        """
        pass

    @enum.setter
    @abstractmethod
    def enum(self, value: Optional[List[Any]]):
        """
        Abstract setter for setting the enum list of the parameter.
        """
        pass

```

```swarmauri/core/utils/__init__.py



```

```swarmauri/core/utils/ITransactional.py

from abc import ABC, abstractmethod

class ITransactional(ABC):

    @abstractmethod
    def begin_transaction(self):
        """
        Initiates a transaction for a series of vector store operations.
        """
        pass
    
    @abstractmethod
    def commit_transaction(self):
        """
        Commits the current transaction, making all operations within the transaction permanent.
        """
        pass
    
    @abstractmethod
    def abort_transaction(self):
        """
        Aborts the current transaction, reverting all operations performed within the transaction.
        """
        pass

```

```swarmauri/core/vector_stores/ISimiliarityQuery.py

from abc import ABC, abstractmethod
from typing import List, Dict

class ISimilarityQuery(ABC):
    
    @abstractmethod
    def search_by_similarity_threshold(self, query_vector: List[float], similarity_threshold: float, space_name: str = None) -> List[Dict]:
        """
        Search vectors exceeding a similarity threshold to a query vector within an optional vector space.

        Args:
            query_vector (List[float]): The high-dimensional query vector.
            similarity_threshold (float): The similarity threshold for filtering results.
            space_name (str, optional): The name of the vector space to search within.

        Returns:
            List[Dict]: A list of dictionaries with vector IDs, similarity scores, and optional metadata that meet the similarity threshold.
        """
        pass

```

```swarmauri/core/vector_stores/IGradient.py

from abc import ABC, abstractmethod
from typing import List, Callable

class IGradient(ABC):
    """
    Interface for calculating the gradient of a scalar field.
    """

    @abstractmethod
    def calculate_gradient(self, scalar_field: Callable[[List[float]], float], point: List[float]) -> List[float]:
        """
        Calculate the gradient of a scalar field at a specific point.

        Parameters:
        - scalar_field (Callable[[List[float]], float]): The scalar field represented as a function
                                                         that takes a point and returns a scalar value.
        - point (List[float]): The point at which the gradient is to be calculated.

        Returns:
        - List[float]: The gradient vector at the specified point.
        """
        pass

```

```swarmauri/core/vector_stores/IAngleBetweenVectors.py

from abc import ABC, abstractmethod
from typing import List

class IAngleBetweenVectors(ABC):
    """
    Interface for calculating the angle between two vectors.
    """

    @abstractmethod
    def angle_between(self, vector_a: List[float], vector_b: List[float]) -> float:
        """
        Method to calculate and return the angle in radians between two vectors.

        Parameters:
        - vector_a (List[float]): The first vector as a list of floats.
        - vector_b (List[float]): The second vector as a list of floats.

        Returns:
        - float: The angle between vector_a and vector_b in radians.

        Note: Implementations should handle the vectors' dimensionality and throw appropriate exceptions for incompatible vectors.
        """
        pass

```

```swarmauri/core/vector_stores/IDecompose.py

from abc import ABC, abstractmethod
from typing import Tuple, List
from .IVector import IVector  # Assuming there's a base IVector interface for vector representations

class IDecompose(ABC):
    """
    Interface for decomposing a vector into components along specified basis vectors.
    This operation is essential in expressing a vector in different coordinate systems or reference frames.
    """

    @abstractmethod
    def decompose(self, vector: IVector, basis_vectors: List[IVector]) -> List[IVector]:
        """
        Decompose the given vector into components along the specified basis vectors.

        Parameters:
        - vector (IVector): The vector to be decomposed.
        - basis_vectors (List[IVector]): A list of basis vectors along which to decompose the given vector.

        Returns:
        - List[IVector]: A list of vectors, each representing the component of the decomposed vector along 
                         the corresponding basis vector in the `basis_vectors` list.
        """
        pass

```

```swarmauri/core/vector_stores/IDivergence.py

from abc import ABC, abstractmethod
from typing import List

class IDivergence(ABC):
    """
    Interface for calculating the divergence of a vector field.
    """

    @abstractmethod
    def calculate_divergence(self, vector_field: List[List[float]], point: List[float]) -> float:
        """
        Calculate the divergence of a vector field at a specific point.

        Parameters:
        - vector_field (List[List[float]]): A representation of the vector field as a list of vectors.
        - point (List[float]): The point at which the divergence is to be calculated.

        Returns:
        - float: The divergence value at the specified point.
        """
        pass

```

```swarmauri/core/vector_stores/IOrthogonalProject.py

from abc import ABC, abstractmethod
from typing import List

class IOrthogonalProject(ABC):
    """
    Interface for calculating the orthogonal projection of one vector onto another.
    """

    @abstractmethod
    def orthogonal_project(self, vector_a: List[float], vector_b: List[float]) -> List[float]:
        """
        Calculates the orthogonal projection of vector_a onto vector_b.
        
        Args:
            vector_a (List[float]): The vector to be projected.
            vector_b (List[float]): The vector onto which vector_a is orthogonally projected.
        
        Returns:
            List[float]: The orthogonal projection of vector_a onto vector_b.
        """
        pass

```

```swarmauri/core/vector_stores/IProject.py

from abc import ABC, abstractmethod
from typing import List

class IProject(ABC):
    """
    Interface for projecting one vector onto another.
    """

    @abstractmethod
    def project(self, vector_a: List[float], vector_b: List[float]) -> List[float]:
        """
        Projects vector_a onto vector_b.
        
        Args:
            vector_a (List[float]): The vector to be projected.
            vector_b (List[float]): The vector onto which vector_a is projected.
        
        Returns:
            List[float]: The projection of vector_a onto vector_b.
        """
        pass



```

```swarmauri/core/vector_stores/IReflect.py

from abc import ABC, abstractmethod
from typing import List

class IReflect(ABC):
    """
    Interface for reflecting a vector across a specified plane or axis.
    """

    @abstractmethod
    def reflect_vector(self, vector: List[float], normal: List[float]) -> List[float]:
        """
        Reflects a vector across a plane or axis defined by a normal vector.

        Parameters:
        - vector (List[float]): The vector to be reflected.
        - normal (List[float]): The normal vector of the plane across which the vector will be reflected.

        Returns:
        - List[float]: The reflected vector.
        """
        pass

```

```swarmauri/core/vector_stores/ISimilarity.py

from abc import ABC, abstractmethod
from typing import List, Tuple
from .IVector import IVector

class ISimilarity(ABC):
    """
    Interface to define operations for computing similarity and distance between vectors.
    This interface is crucial for systems that need to perform similarity searches, clustering,
    or any operations where vector similarity plays a key role.
    """

    @abstractmethod
    def similarity(self, vector_a: IVector, vector_b: IVector) -> float:
        """
        Compute the similarity between two vectors. The definition of similarity (e.g., cosine similarity)
        should be implemented in concrete classes.

        Args:
            vector_a (IVector): The first vector.
            vector_b (IVector): The second vector to compare with the first vector.

        Returns:
            float: A similarity score between vector_a and vector_b.
        """
        pass



```

```swarmauri/core/vector_stores/IVectorSpan.py

from abc import ABC, abstractmethod
from typing import List, Any

class IVectorSpan(ABC):
    """
    Interface for determining if a vector is within the span of a set of vectors.
    """

    @abstractmethod
    def in_span(self, vector: Any, basis_vectors: List[Any]) -> bool:
        """
        Checks if the given vector is in the span of the provided basis vectors.

        Parameters:
        - vector (Any): The vector to check.
        - basis_vectors (List[Any]): A list of vectors that might span the vector.

        Returns:
        - bool: True if the vector is in the span of the basis_vectors, False otherwise.
        """
        pass

```

```swarmauri/core/vector_stores/IVectorArithmetic.py

from abc import ABC, abstractmethod
from typing import List

class IVectorArithmetic(ABC):
    @abstractmethod
    def add(self, vector1: List[float], vector2: List[float]) -> List[float]:
        """
        Vector addition of 'vector1' and 'vector2'.
        """
        pass
        
    @abstractmethod
    def subtract(self, vector1: List[float], vector2: List[float]) -> List[float]:
        """
        Vector subtraction of 'vector1' - 'vector2'.
        """
        pass
   
    @abstractmethod
    def multiply(self, vector: List[float], scalar: float) -> List[float]:
        """
        Scalar multiplication of 'vector' by 'scalar'.
        """
        pass
        
    @abstractmethod
    def divide(self, vector: List[float], scalar: float) -> List[float]:
        """
        Scalar division of 'vector' by 'scalar'.
        """
        pass

```

```swarmauri/core/vector_stores/IVectorLinearCombination.py

from abc import ABC, abstractmethod
from typing import List, Any

class ILinearCombination(ABC):
    """
    Interface for creating a vector as a linear combination of a set of vectors.
    """

    @abstractmethod
    def linear_combination(self, coefficients: List[float], vectors: List[Any]) -> Any:
        """
        Computes the linear combination of the given vectors with the specified coefficients.

        Parameters:
        - coefficients (List[float]): A list of coefficients for the linear combination.
        - vectors (List[Any]): A list of vectors to be combined.

        Returns:
        - Any: The resulting vector from the linear combination.
        """
        pass

```

```swarmauri/core/vector_stores/IVectorNorm.py

# core/vectors/IVectorNorm.py

from abc import ABC, abstractmethod
from typing import List, Union

class IVectorNorm(ABC):
    """
    Interface for calculating vector norms.
    Supports L1 norm, L2 norm, and Max norm calculations.
    """

    @abstractmethod
    def l1_norm(self, vector: List[Union[int, float]]) -> float:
        """
        Calculate the L1 norm (Manhattan norm) of a vector.

        Parameters:
        - vector (List[Union[int, float]]): The vector for which to calculate the L1 norm.

        Returns:
        - float: The L1 norm of the vector.
        """
        pass

    @abstractmethod
    def l2_norm(self, vector: List[Union[int, float]]) -> float:
        """
        Calculate the L2 norm (Euclidean norm) of a vector.

        Parameters:
        - vector (List[Union[int, float]]): The vector for which to calculate the L2 norm.

        Returns:
        - float: The L2 norm of the vector.
        """
        pass

    @abstractmethod
    def max_norm(self, vector: List[Union[int, float]]) -> float:
        """
        Calculate the Max norm (infinity norm) of a vector.

        Parameters:
        - vector (List[Union[int, float]]): The vector for which to calculate the Max norm.

        Returns:
        - float: The Max norm of the vector.
        """
        pass

```

```swarmauri/core/vector_stores/IVectorProduct.py

from abc import ABC, abstractmethod
from typing import List, Tuple

class IVectorProduct(ABC):
    """
    Interface for various vector products including dot product, cross product,
    and triple products (vector and scalar).
    """

    @abstractmethod
    def dot_product(self, vector_a: List[float], vector_b: List[float]) -> float:
        """
        Calculate the dot product of two vectors.

        Parameters:
        - vector_a (List[float]): The first vector.
        - vector_b (List[float]): The second vector.

        Returns:
        - float: The dot product of the two vectors.
        """
        pass

    @abstractmethod
    def cross_product(self, vector_a: List[float], vector_b: List[float]) -> List[float]:
        """
        Calculate the cross product of two vectors.

        Parameters:
        - vector_a (List[float]): The first vector.
        - vector_b (List[float]): The second vector.

        Returns:
        - List[float]: The cross product as a new vector.
        """
        pass

    @abstractmethod
    def vector_triple_product(self, vector_a: List[float], vector_b: List[float], vector_c: List[float]) -> List[float]:
        """
        Calculate the vector triple product of three vectors.

        Parameters:
        - vector_a (List[float]): The first vector.
        - vector_b (List[float]): The second vector.
        - vector_c (List[float]): The third vector.

        Returns:
        - List[float]: The result of the vector triple product as a new vector.
        """
        pass

    @abstractmethod
    def scalar_triple_product(self, vector_a: List[float], vector_b: List[float], vector_c: List[float]) -> float:
        """
        Calculate the scalar triple product of three vectors.

        Parameters:
        - vector_a (List[float]): The first vector.
        - vector_b (List[float]): The second vector.
        - vector_c (List[float]): The third vector.

        Returns:
        - float: The scalar value result of the scalar triple product.
        """
        pass

```

```swarmauri/core/vector_stores/IVectorRotate.py

from abc import ABC, abstractmethod
from typing import List

class IRotate(ABC):
    """
    Interface for rotating a vector.
    """
    
    @abstractmethod
    def rotate(self, vector: List[float], angle: float, axis: List[float] = None) -> List[float]:
        """
        Rotate the given vector by a specified angle around an axis (for 3D) or in a plane (for 2D).

        For 2D vectors, the axis parameter can be omitted.

        Args:
            vector (List[float]): The vector to rotate.
            angle (float): The angle of rotation in degrees.
            axis (List[float], optional): The axis of rotation (applicable in 3D).

        Returns:
            List[float]: The rotated vector.
        """
        pass


```

```swarmauri/core/vector_stores/IVectorBasisCheck.py

from abc import ABC, abstractmethod
from typing import List, Any

class IVectorBasisCheck(ABC):
    """
    Interface for checking if a given set of vectors forms a basis of the vector space.
    """

    @abstractmethod
    def is_basis(self, vectors: List[Any]) -> bool:
        """
        Determines whether the given set of vectors forms a basis for their vector space.

        Parameters:
        - vectors (List[Any]): A list of vectors to be checked.

        Returns:
        - bool: True if the vectors form a basis, False otherwise.
        """
        pass

```

```swarmauri/core/vector_stores/IVectorStore.py

from abc import ABC, abstractmethod
from typing import List, Dict, Union
from ..vectors.IVector import IVector

class IVectorStore(ABC):
    """
    Interface for a vector store that allows the storage, retrieval,
    and management of high-dimensional vector data used in search and machine learning applications.
    """

    @abstractmethod
    def add_vector(self, vector_id: str, vector: IVector, metadata: Dict = None) -> None:
        """
        Store a vector along with its identifier and optional metadata.

        Args:
            vector_id (str): Unique identifier for the vector.
            vector (List[float]): The high-dimensional vector to be stored.
            metadata (Dict, optional): Optional metadata related to the vector.
        """
        pass

    @abstractmethod
    def get_vector(self, vector_id: str) -> Union[List[float], None]:
        """
        Retrieve a vector by its identifier.

        Args:
            vector_id (str): The unique identifier for the vector.

        Returns:
            Union[List[float], None]: The vector associated with the given ID, or None if not found.
        """
        pass

    @abstractmethod
    def delete_vector(self, vector_id: str) -> None:
        """
        Delete a vector by its identifier.

        Args:
            vector_id (str): The unique identifier for the vector to be deleted.
        """
        pass

    @abstractmethod
    def update_vector(self, vector_id: str, new_vector: IVector, new_metadata: Dict = None) -> None:
        """
        Update the vector and metadata for a given vector ID.

        Args:
            vector_id (str): The unique identifier for the vector to update.
            new_vector (List[float]): The new vector data to store.
            new_metadata (Dict, optional): Optional new metadata related to the vector.
        """
        pass

```

```swarmauri/core/vector_stores/IDistanceSimilarity.py

from abc import ABC, abstractmethod
from typing import List
from ..vectors.IVector import IVector

class IDistanceSimilarity(ABC):
    """
    Interface for computing distances and similarities between high-dimensional data vectors. This interface
    abstracts the method for calculating the distance and similarity, allowing for the implementation of various 
    distance metrics such as Euclidean, Manhattan, Cosine similarity, etc.
    """

    @abstractmethod
    def distance(self, vector_a: IVector, vector_b: IVector) -> float:
        """
        Computes the distance between two vectors.

        Args:
            vector_a (IVector): The first vector in the comparison.
            vector_b (IVector): The second vector in the comparison.

        Returns:
            float: The computed distance between vector_a and vector_b.
        """
        pass
    

    @abstractmethod
    def distances(self, vector_a: IVector, vectors_b: List[IVector]) -> float:
        pass


    @abstractmethod
    def similarity(self, vector_a: IVector, vector_b: IVector) -> float:
        """
        Compute the similarity between two vectors. The definition of similarity (e.g., cosine similarity)
        should be implemented in concrete classes.

        Args:
            vector_a (IVector): The first vector.
            vector_b (IVector): The second vector to compare with the first vector.

        Returns:
            float: A similarity score between vector_a and vector_b.
        """
        pass

    @abstractmethod
    def similarities(self, vector_a: IVector, vectors_b: List[IVector]) -> float:
        pass


```

```swarmauri/core/vector_stores/__init__.py



```

```swarmauri/core/document_stores/IDocumentStore.py

from abc import ABC, abstractmethod
from typing import List, Union
from ..documents.IDocument import IDocument

class IDocumentStore(ABC):
    """
    Interface for a Document Store responsible for storing, indexing, and retrieving documents.
    """

    @abstractmethod
    def add_document(self, document: IDocument) -> None:
        """
        Stores a single document in the document store.

        Parameters:
        - document (IDocument): The document to store.
        """
        pass

    @abstractmethod
    def add_documents(self, documents: List[IDocument]) -> None:
        """
        Stores multiple documents in the document store.

        Parameters:
        - documents (List[IDocument]): The list of documents to store.
        """
        pass

    @abstractmethod
    def get_document(self, doc_id: str) -> Union[IDocument, None]:
        """
        Retrieves a document by its ID.

        Parameters:
        - doc_id (str): The unique identifier for the document.

        Returns:
        - Union[IDocument, None]: The requested document, or None if not found.
        """
        pass

    @abstractmethod
    def get_all_documents(self) -> List[IDocument]:
        """
        Retrieves all documents stored in the document store.

        Returns:
        - List[IDocument]: A list of all documents.
        """
        pass

    @abstractmethod
    def delete_document(self, doc_id: str) -> None:
        """
        Deletes a document from the document store by its ID.

        Parameters:
        - doc_id (str): The unique identifier of the document to delete.
        """
        pass


    @abstractmethod
    def update_document(self, doc_id: str, updated_document: IDocument) -> None:
        """
        Updates a document in the document store.

        Parameters:
        - doc_id (str): The unique identifier for the document to update.
        - updated_document (IDocument): The updated document object.

        Note: It's assumed that the updated_document will retain the same doc_id but may have different content or metadata.
        """
        pass

    @abstractmethod
    def document_count(self) -> int:
        pass

```

```swarmauri/core/document_stores/__init__.py



```

```swarmauri/core/document_stores/IDocumentRetrieve.py

from abc import ABC, abstractmethod
from typing import List
from swarmauri.core.documents.IDocument import IDocument

class IDocumentRetrieve(ABC):
    """
    Abstract base class for document retrieval operations.
    
    This class defines the interface for retrieving documents based on a query or other criteria.
    Implementations may use various indexing or search technologies to fulfill these retrievals.
    """

    @abstractmethod
    def retrieve(self, query: str, top_k: int = 5) -> List[IDocument]:
        """
        Retrieve the most relevant documents based on the given query.
        
        Parameters:
            query (str): The query string used for document retrieval.
            top_k (int): The number of top relevant documents to retrieve.
            
        Returns:
            List[Document]: A list of the top_k most relevant documents.
        """
        pass

```

```swarmauri/core/chunkers/__init__.py

# -*- coding: utf-8 -*-
"""
Created on Wed Feb 28 20:35:27 2024

@author: bigman
"""



```

```swarmauri/core/chunkers/IChunker.py

from abc import ABC, abstractmethod
from typing import List, Union, Any

class IChunker(ABC):
    """
    Interface for chunking text into smaller pieces.

    This interface defines abstract methods for chunking texts. Implementing classes
    should provide concrete implementations for these methods tailored to their specific
    chunking algorithms.
    """

    @abstractmethod
    def chunk_text(self, text: Union[str, Any], *args, **kwargs) -> List[Any]:
        pass

```

```swarmauri/core/vectors/IVectorMeta.py

from abc import ABC, abstractmethod
from typing import Any, Dict, List

class IVectorMeta(ABC):
    """
    Interface for a high-dimensional data vector. This interface defines the
    basic structure and operations for interacting with vectors in various applications,
    such as machine learning, information retrieval, and similarity search.
    """

    @property
    @abstractmethod
    def id(self) -> str:
        """
        Unique identifier for the vector. This ID can be used to reference the vector
        in a database or a vector store.
        """
        pass

    @property
    @abstractmethod
    def metadata(self) -> Dict[str, Any]:
        """
        Optional metadata associated with the vector. Metadata can include additional information
        useful for retrieval, categorization, or description of the vector data.
        """
        pass



```

```swarmauri/core/vectors/IVectorTransform.py

from abc import ABC, abstractmethod
from .IVector import IVector

class IVectorTransform(ABC):
    """
    Interface for performing various transformations on vectors.
    """

    @abstractmethod
    def translate(self, translation_vector: IVector) -> IVector:
        """
        Translate a vector by a given translation vector.
        """
        pass

    @abstractmethod
    def rotate(self, angle: float, axis: IVector) -> IVector:
        """
        Rotate a vector around a given axis by a certain angle.
        """
        pass

    @abstractmethod
    def reflect(self, plane_normal: IVector) -> IVector:
        """
        Reflect a vector across a plane defined by its normal vector.
        """
        pass

    @abstractmethod
    def scale(self, scale_factor: float) -> IVector:
        """
        Scale a vector by a given scale factor.
        """
        pass

    @abstractmethod
    def shear(self, shear_factor: float, direction: IVector) -> IVector:
        """
        Shear a vector along a given direction by a shear factor.
        """
        pass

    @abstractmethod
    def project(self, plane_normal: IVector) -> IVector:
        """
        Project a vector onto a plane defined by its normal vector.
        """
        pass

```

```swarmauri/core/vectors/IVector.py

from abc import ABC, abstractmethod
from typing import Any, Dict, List

class IVector(ABC):
    """
    Interface for a high-dimensional data vector. This interface defines the
    basic structure and operations for interacting with vectors in various applications,
    such as machine learning, information retrieval, and similarity search.
    """

    @property
    @abstractmethod
    def data(self) -> List[float]:
        """
        The high-dimensional data that the vector represents. It is typically a list of float values.
        """
        pass



```

```swarmauri/core/vectors/__init__.py



```

```swarmauri/core/swarm_apis/__init__.py



```

```swarmauri/core/swarm_apis/ISwarmAPI.py

from abc import ABC, abstractmethod
from typing import List, Dict, Any

class ISwarmAPI(ABC):
    """
    Interface for managing the swarm's API endpoints.
    """
    
    @abstractmethod
    def list_agents(self) -> List[Dict[str, Any]]:
        """
        Lists all registered agents within the swarm.

        Returns:
        - List[Dict[str, Any]]: A list of dictionaries containing information about each agent.
        """
        pass

    @abstractmethod
    def get_swarm_capabilities(self) -> List[str]:
        """
        Retrieves a list of all unique capabilities supported by the swarm.

        Returns:
        - List[str]: A list of unique capabilities.
        """
        pass

    @abstractmethod
    def dispatch_request(self, request_data: Dict[str, Any]) -> Any:
        """
        Dispatches an incoming user request to one or more suitable agents based on their capabilities.

        Parameters:
        - request_data (Dict[str, Any]): Data related to the incoming request.

        Returns:
        - Any: Response from processing the request.
        """
        pass

```

```swarmauri/core/swarm_apis/IAgentRegistration.py

from abc import ABC, abstractmethod
from typing import List, Dict, Optional
from ...agents.ISwarmAgent import ISwarmAgent

class IAgentRegistration(ABC):
    """
    Interface for registering agents with the swarm, designed to support CRUD operations on ISwarmAgent instances.
    """

    @abstractmethod
    def register_agent(self, agent: ISwarmAgent) -> bool:
        """
        Register a new agent with the swarm.

        Parameters:
            agent (ISwarmAgent): An instance of ISwarmAgent representing the agent to register.

        Returns:
            bool: True if the registration succeeded; False otherwise.
        """
        pass

    @abstractmethod
    def update_agent(self, agent_id: str, updated_agent: ISwarmAgent) -> bool:
        """
        Update the details of an existing agent. This could include changing the agent's configuration,
        task assignment, or any other mutable attribute.

        Parameters:
            agent_id (str): The unique identifier for the agent.
            updated_agent (ISwarmAgent): An updated ISwarmAgent instance to replace the existing one.

        Returns:
            bool: True if the update was successful; False otherwise.
        """
        pass

    @abstractmethod
    def remove_agent(self, agent_id: str) -> bool:
        """
        Remove an agent from the swarm based on its unique identifier.

        Parameters:
            agent_id (str): The unique identifier for the agent to be removed.

        Returns:
            bool: True if the removal was successful; False otherwise.
        """
        pass

    @abstractmethod
    def get_agent(self, agent_id: str) -> Optional[ISwarmAgent]:
        """
        Retrieve an agent's instance from its unique identifier.

        Parameters:
            agent_id (str): The unique identifier for the agent of interest.

        Returns:
            Optional[ISwarmAgent]: The ISwarmAgent instance if found; None otherwise.
        """
        pass

    @abstractmethod
    def list_agents(self) -> List[ISwarmAgent]:
        """
        List all registered agents.

        Returns:
            List[ISwarmAgent]: A list containing instances of all registered ISwarmAgents.
        """
        pass

```

```swarmauri/core/vectorizers/__init__.py

#

```

```swarmauri/core/vectorizers/IVectorize.py

from abc import ABC, abstractmethod
from typing import List, Union, Any
from swarmauri.core.vectors.IVector import IVector

class IVectorize(ABC):
    """
    Interface for converting text to vectors. 
    Implementations of this interface transform input text into numerical 
    vectors that can be used in machine learning models, similarity calculations, 
    and other vector-based operations.
    """
    @abstractmethod
    def fit(self, data: Union[str, Any]) -> List[IVector]:
        pass
    
    @abstractmethod
    def transform(self, data: Union[str, Any]) -> List[IVector]:
        pass

    @abstractmethod
    def fit_transform(self, data: Union[str, Any]) -> List[IVector]:
        pass

    @abstractmethod
    def infer_vector(self, data: Union[str, Any], *args, **kwargs) -> IVector:
        pass

```

```swarmauri/core/vectorizers/IFeature.py

from abc import ABC, abstractmethod
from typing import List, Any

class IFeature(ABC):

    @abstractmethod
    def extract_features(self) -> List[Any]:
        pass
    


```

```swarmauri/core/tracing/__init__.py



```

```swarmauri/core/tracing/ITraceContext.py

from abc import ABC, abstractmethod
from typing import Any

class ITraceContext(ABC):
    """
    Interface for a trace context, representing a single trace instance.
    This context carries the state and metadata of the trace across different system components.
    """

    @abstractmethod
    def get_trace_id(self) -> str:
        """
        Retrieves the unique identifier for this trace.

        Returns:
            str: The unique trace identifier.
        """
        pass

    @abstractmethod
    def add_attribute(self, key: str, value: Any):
        """
        Adds or updates an attribute associated with this trace.

        Args:
            key (str): The attribute key or name.
            value (Any): The value of the attribute.
        """
        pass

```

```swarmauri/core/tracing/ITracer.py

from swarmauri.core.tracing.ITraceContext import ITraceContext
from abc import ABC, abstractmethod
from typing import Optional, Dict, Any


class ITracer(ABC):
    """
    Interface for implementing distributed tracing across different components of the system.
    """

    @abstractmethod
    def start_trace(self, name: str, initial_attributes: Optional[Dict[str, Any]] = None) -> ITraceContext:
        """
        Starts a new trace with a given name and optional initial attributes.

        Args:
            name (str): Name of the trace, usually represents the operation being traced.
            initial_attributes (Optional[Dict[str, Any]]): Key-value pairs to be attached to the trace initially.

        Returns:
            ITraceContext: A context object representing this particular trace instance.
        """
        pass

    @abstractmethod
    def end_trace(self, trace_context: ITraceContext):
        """
        Marks the end of a trace, completing its lifecycle and recording its details.

        Args:
            trace_context (ITraceContext): The trace context to be ended.
        """
        pass

    @abstractmethod
    def annotate_trace(self, trace_context: ITraceContext, key: str, value: Any):
        """
        Adds an annotation to an existing trace, enriching it with more detailed information.

        Args:
            trace_context (ITraceContext): The trace context to annotate.
            key (str): The key or name of the annotation.
            value (Any): The value of the annotation.
        """
        pass

```

```swarmauri/core/tracing/IChainTracer.py

from abc import ABC, abstractmethod
from typing import Callable, List, Tuple, Dict, Any

class IChainTracer(ABC):
    """
    Interface for a tracer supporting method chaining through a list of tuples.
    Each tuple in the list contains: trace context, function, args, and kwargs.
    """

    @abstractmethod
    def process_chain(self, chain: List[Tuple[Any, Callable[..., Any], List[Any], Dict[str, Any]]]) -> "IChainTracer":
        """
        Processes a sequence of operations defined in a chain.

        Args:
            chain (List[Tuple[Any, Callable[..., Any], List[Any], Dict[str, Any]]]): A list where each tuple contains:
                - The trace context or reference required by the function.
                - The function (method of IChainTracer) to execute.
                - A list of positional arguments for the function.
                - A dictionary of keyword arguments for the function.

        Returns:
            IChainTracer: Returns self to allow further method chaining.
        """
        pass

```

```swarmauri/core/chains/ICallableChain.py

from abc import ABC, abstractmethod
from typing import Any, Callable, List, Tuple

CallableDefinition = Tuple[Callable, List[Any], dict]

class ICallableChain(ABC):
    @abstractmethod
    def __call__(self, *initial_args: Any, **initial_kwargs: Any) -> Any:
        """Executes the chain of callables."""
        pass

    @abstractmethod
    def add_callable(self, func: Callable, args: List[Any] = None, kwargs: dict = None) -> None:
        """Adds a new callable to the chain."""
        pass

```

```swarmauri/core/chains/__init__.py

from swarmauri.core.chains.ICallableChain import ICallableChain

```

```swarmauri/core/chains/IChain.py

from abc import ABC, abstractmethod
from typing import List, Any, Dict
from swarmauri.core.chains.IChainStep import IChainStep
from swarmauri.experimental.chains.IChainOrderStrategy import IChainOrderStrategy
from swarmauri.experimental.chains.IChainProcessingStrategy import IChainProcessingStrategy

class IChain(ABC):
    """
    Interface for managing execution chains within the system.
    """

    @abstractmethod
    def __init__(self, order_strategy: IChainOrderStrategy, processing_strategy: IChainProcessingStrategy, steps: List[IChainStep] = None, **configs):
        pass

    @abstractmethod
    def add_step(self, step: IChainStep):
        pass
    

    @abstractmethod
    def invoke(self, *args, **kwargs) -> Any:
        pass

    @abstractmethod
    async def ainvoke(self, *args, **kwargs) -> Any:
        pass

    @abstractmethod
    def batch(self, requests: List[Dict[str, Any]]) -> List[Any]:
        pass

    @abstractmethod
    async def abatch(self, requests: List[Dict[str, Any]]) -> List[Any]:
        pass

    @abstractmethod
    def stream(self, *args, **kwargs) -> Any:
        pass

    @abstractmethod
    def get_schema_info(self) -> Dict[str, Any]:
        pass

```

```swarmauri/core/chains/IChainFactory.py

from abc import ABC, abstractmethod
from subprocess import ABOVE_NORMAL_PRIORITY_CLASS
from typing import List, Any, Dict
from swarmauri.core.chains.IChain import IChain
from swarmauri.core.chains.IChainStep import IChainStep
from swarmauri.experimental.chains.IChainOrderStrategy import IChainOrderStrategy
from swarmauri.experimental.chains.IChainProcessingStrategy import IChainProcessingStrategy

class IChainFactory(ABC):
    """
    Interface for creating and managing execution chains within the system.
    """

    @abstractmethod
    def __init__(self, order_strategy: IChainOrderStrategy, processing_strategy: IChainProcessingStrategy, **configs):
        pass

    @abstractmethod
    def create_chain(self, steps: List[IChainStep] = None) -> IChain:
        pass
    
    @abstractmethod
    def get_schema_info(self) -> Dict[str, Any]:
        pass
    
    @abstractmethod
    def get_chain_info(self) -> Dict[str, Any]:
        pass
    
    @abstractmethod
    def get_chain(self) -> IChain:
        pass
    
    @abstractmethod
    def set_chain(self, chain: IChain):
        pass
    
    @abstractmethod
    def reset_chain(self):
        pass
    
    @abstractmethod
    def get_chain_steps(self) -> List[IChainStep]:
        pass
    
    @abstractmethod
    def set_chain_steps(self, steps: List[IChainStep]):
        pass
    
    @abstractmethod
    def add_chain_step(self, step: IChainStep):
        pass
    
    @abstractmethod
    def remove_chain_step(self, key: str):
        pass
    
    @abstractmethod
    def get_chain_order_strategy(self) -> IChainOrderStrategy:
        pass
    
    @abstractmethod
    def set_chain_order_strategy(self, order_strategy: IChainOrderStrategy):
        pass
    
    @abstractmethod
    def get_chain_processing_strategy(self) -> IChainProcessingStrategy:
        pass
    
    @abstractmethod
    def set_chain_processing_strategy(self, processing_strategy: IChainProcessingStrategy):
        pass
    
    @abstractmethod
    def get_configs(self) -> Dict[str, Any]:
        pass
    
    @abstractmethod
    def set_configs(self, **configs):
        pass
    
    @abstractmethod
    
    def get_config(self, key: str) -> Any:
        pass
    
    @abstractmethod
    def set_config(self, key: str, value: Any):
        pass
    
    @abstractmethod
    def get_schema_info(self) -> Dict[str, Any]:
        pass

    @abstractmethod
    def get_chain_info(self) -> Dict[str, Any]:
        pass    
    


```

```swarmauri/core/chains/IChainStep.py

from typing import List, Dict, Any, Callable

class IChainStep:
    """
    Represents a single step within an execution chain.
    """
    def __init__(self, key: str, method: Callable, args: List[Any] = None, kwargs: Dict[str, Any] = None, ref: str = None):
        """
        Initialize a chain step.

        Args:
            key (str): Unique key or identifier for the step.
            method (Callable): The callable object (function or method) to execute in this step.
            args (List[Any], optional): Positional arguments for the callable.
            kwargs (Dict[str, Any], optional): Keyword arguments for the callable.
            ref (str, optional): Reference to another component or context variable, if applicable.
        """
        self.key = key
        self.method = method
        self.args = args if args is not None else []
        self.kwargs = kwargs if kwargs is not None else {}
        self.ref = ref

```
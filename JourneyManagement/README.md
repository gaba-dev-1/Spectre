<div align="center">

# 🗺️ Journey Management System

**Multi-stage voyage planning with Homeric wisdom**

[![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)]()
[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)]()

*Plan and manage your odyssey through the flooded world*

</div>

### Implementation

```typescript
import React, { useState, useEffect } from 'react';
import { Canvas } from '@react-three/fiber';
import { OrbitControls, Text, Box, Sphere } from '@react-three/drei';
import * as THREE from 'three';

interface JourneyStage {
  id: string;
  name: string;
  description: string;
  position: THREE.Vector3;
  estimatedDuration: number;
  requiredResources: Resource[];
  hazardLevel: number;
  homericParallel: string;
  completionCriteria: string[];
}

interface Resource {
  type: 'food' | 'water' | 'fuel' | 'medicine' | 'tools';
  amount: number;
  unit: string;
  priority: 'critical' | 'important' | 'optional';
}

interface OdysseyPlan {
  stages: JourneyStage[];
  totalDistance: number;
  estimatedDuration: number;
  riskAssessment: RiskAssessment;
  resourceRequirements: Resource[];
  contingencyPlans: ContingencyPlan[];
}

const JourneyPlanningSystem: React.FC = () => {
  const [currentPlan, setCurrentPlan] = useState<OdysseyPlan | null>(null);
  const [activeStage, setActiveStage] = useState<string | null>(null);
  const [journeyProgress, setJourneyProgress] = useState<number>(0);
  const [homericGuidance, setHomericGuidance] = useState<string>('');

  useEffect(() => {
    loadHomericWisdom();
    initializeJourneyPlanning();
  }, []);

  const loadHomericWisdom = () => {
    const wisdom = [
      "Like Odysseus, the journey home tests not just your strength, but your wisdom",
      "Each island of safety is a lesson; don't linger too long in comfort",
      "The sea may be vast, but with purpose and patience, you will find your Ithaca",
      "Trust your crew, but verify your course - even gods can mislead",
      "When storms rage, remember: this too shall pass, as it did for the Son of Laertes"
    ];
    
    setHomericGuidance(wisdom[Math.floor(Math.random() * wisdom.length)]);
  };

  const createOdysseyPlan = (
    startPosition: THREE.Vector3,
    destination: THREE.Vector3,
    parameters: JourneyParameters
  ): OdysseyPlan => {
    
    const stages = generateJourneyStages(startPosition, destination, parameters);
    const resources = calculateResourceRequirements(stages);
    const risks = assessJourneyRisks(stages);
    const contingencies = createContingencyPlans(stages, risks);

    return {
      stages,
      totalDistance: calculateTotalDistance(stages),
      estimatedDuration: calculateTotalDuration(stages),
      riskAssessment: risks,
      resourceRequirements: resources,
      contingencyPlans: contingencies
    };
  };

  const generateJourneyStages = (
    start: THREE.Vector3,
    end: THREE.Vector3,
    params: JourneyParameters
  ): JourneyStage[] => {
    
    const stages: JourneyStage[] = [];
    
    // Generate stages based on Odyssey structure
    stages.push({
      id: 'departure',
      name: 'Departure from Troy',
      description: 'Leave the safety of current location',
      position: start,
      estimatedDuration: 0.5,
      requiredResources: [
        { type: 'food', amount: 3, unit: 'days', priority: 'critical' },
        { type: 'water', amount: 5, unit: 'liters', priority: 'critical' }
      ],
      hazardLevel: 0.2,
      homericParallel: 'The Greeks departing Troy after victory',
      completionCriteria: ['All supplies loaded', 'Route confirmed', 'Weather checked']
    });

    // Add intermediate stages (like islands in the Odyssey)
    const intermediateStages = generateIntermediateStages(start, end, params);
    stages.push(...intermediateStages);

    stages.push({
      id: 'homecoming',
      name: 'Return to Ithaca',
      description: 'Reach the final destination safely',
      position: end,
      estimatedDuration: 1,
      requiredResources: [],
      hazardLevel: 0.1,
      homericParallel: 'Odysseus finally reaching Ithaca after 20 years',
      completionCriteria: ['Safe arrival', 'All companions accounted for', 'Establishment of shelter']
    });

    return stages;
  };

  const generateIntermediateStages = (
    start: THREE.Vector3,
    end: THREE.Vector3,
    params: JourneyParameters
  ): JourneyStage[] => {
    
    const stages: JourneyStage[] = [];
    const direction = end.clone().sub(start).normalize();
    const distance = start.distanceTo(end);
    const numStages = Math.ceil(distance / params.maxStageDistance);

    const homericIslands = [
      {
        name: 'Land of the Lotus Eaters',
        parallel: 'Temporary comfort that delays the journey',
        hazard: 0.3,
        resources: [{ type: 'food', amount: 2, unit: 'days', priority: 'important' }]
      },
      {
        name: 'Cyclops Cave',
        parallel: 'Dangerous obstacle requiring cunning to overcome',
        hazard: 0.8,
        resources: [{ type: 'tools', amount: 1, unit: 'set', priority: 'critical' }]
      },
      {
        name: 'Island of Aeolus',
        parallel: 'Helpful encounter with unpredictable consequences',
        hazard: 0.4,
        resources: [{ type: 'fuel', amount: 10, unit: 'liters', priority: 'important' }]
      },
      {
        name: 'Circe\'s Island',
        parallel: 'Transformation and wisdom gained through trials',
        hazard: 0.5,
        resources: [{ type: 'medicine', amount: 3, unit: 'doses', priority: 'important' }]
      },
      {
        name: 'Strait of Scylla and Charybdis',
        parallel: 'Choosing between two dangerous options',
        hazard: 0.9,
        resources: [{ type: 'fuel', amount: 15, unit: 'liters', priority: 'critical' }]
      }
    ];

    for (let i = 1; i < numStages; i++) {
      const progress = i / numStages;
      const position = start.clone().add(direction.clone().multiplyScalar(distance * progress));
      const island = homericIslands[i % homericIslands.length];

      stages.push({
        id: `stage_${i}`,
        name: island.name,
        description: `Navigate through ${island.name.toLowerCase()}`,
        position,
        estimatedDuration: calculateStageDuration(distance / numStages, island.hazard),
        requiredResources: island.resources as Resource[],
        hazardLevel: island.hazard,
        homericParallel: island.parallel,
        completionCriteria: [
          'Navigate hazards successfully',
          'Maintain resource levels',
          'Learn from the experience'
        ]
      });
    }

    return stages;
  };

  const JourneyVisualization: React.FC<{ plan: OdysseyPlan }> = ({ plan }) => (
    <Canvas camera={{ position: [0, 10, 10], fov: 60 }}>
      <ambientLight intensity={0.6} />
      <pointLight position={[10, 10, 10]} />
      <OrbitControls />
      
      {/* Render journey stages */}
      {plan.stages.map((stage, index) => (
        <group key={stage.id} position={stage.position}>
          <Sphere
            args={[0.5]}
            onClick={() => setActiveStage(stage.id)}
          >
            <meshStandardMaterial 
              color={getStageColor(stage.hazardLevel)}
              opacity={activeStage === stage.id ? 1 : 0.7}
              transparent
            />
          </Sphere>
          
          <Text
            position={[0, 1, 0]}
            fontSize={0.3}
            color="white"
            anchorX="center"
            anchorY="middle"
          >
            {stage.name}
          </Text>
          
          {/* Connect stages with lines */}
          {index < plan.stages.length - 1 && (
            <line>
              <bufferGeometry>
                <bufferAttribute
                  attach="attributes-position"
                  count={2}
                  array={new Float32Array([
                    stage.position.x, stage.position.y, stage.position.z,
                    plan.stages[index + 1].position.x,
                    plan.stages[index + 1].position.y,
                    plan.stages[index + 1].position.z
                  ])}
                  itemSize={3}
                />
              </bufferGeometry>
              <lineBasicMaterial color="cyan" />
            </line>
          )}
        </group>
      ))}
      
      {/* Water level visualization */}
      <Box args={[50, 0.1, 50]} position={[0, -2, 0]}>
        <meshStandardMaterial color="blue" opacity={0.3} transparent />
      </Box>
    </Canvas>
  );

  const getStageColor = (hazardLevel: number): string => {
    if (hazardLevel < 0.3) return 'green';
    if (hazardLevel < 0.6) return 'yellow';
    if (hazardLevel < 0.8) return 'orange';
    return 'red';
  };

  const StageDetailsPanel: React.FC<{ stage: JourneyStage }> = ({ stage }) => (
    <div className="stage-details">
      <h3>🏛️ {stage.name}</h3>
      <p><strong>Homeric Parallel:</strong> {stage.homericParallel}</p>
      <p><strong>Description:</strong> {stage.description}</p>
      <p><strong>Estimated Duration:</strong> {stage.estimatedDuration} days</p>
      <p><strong>Hazard Level:</strong> {(stage.hazardLevel * 100).toFixed(0)}%</p>
      
      <div className="resources">
        <h4>Required Resources:</h4>
        <ul>
          {stage.requiredResources.map((resource, index) => (
            <li key={index}>
              {resource.amount} {resource.unit} of {resource.type} 
              <span className={`priority-${resource.priority}`}>
                ({resource.priority})
              </span>
            </li>
          ))}
        </ul>
      </div>
      
      <div className="completion-criteria">
        <h4>Completion Criteria:</h4>
        <ul>
          {stage.completionCriteria.map((criteria, index) => (
            <li key={index}>{criteria}</li>
          ))}
        </ul>
      </div>
    </div>
  );

  const initializeJourneyPlanning = () => {
    // Create a sample journey plan
    const samplePlan = createOdysseyPlan(
      new THREE.Vector3(0, 0, 0),
      new THREE.Vector3(20, 0, 20),
      {
        maxStageDistance: 5,
        riskTolerance: 0.6,
        availableTime: 14,
        groupSize: 4
      }
    );
    
    setCurrentPlan(samplePlan);
  };

  return (
    <div className="journey-planning-system">
      <header className="planning-header">
        <h1>⛵ Odyssey Journey Planning</h1>
        <div className="homeric-wisdom">
          <p>📜 <em>"{homericGuidance}"</em></p>
        </div>
      </header>

      <div className="planning-content">
        <div className="journey-visualization">
          {currentPlan && <JourneyVisualization plan={currentPlan} />}
        </div>

        <div className="planning-controls">
          <div className="journey-overview">
            <h2>🗺️ Journey Overview</h2>
            {currentPlan && (
              <div>
                <p><strong>Total Distance:</strong> {currentPlan.totalDistance.toFixed(1)} km</p>
                <p><strong>Estimated Duration:</strong> {currentPlan.estimatedDuration.toFixed(1)} days</p>
                <p><strong>Risk Level:</strong> {(currentPlan.riskAssessment.overallRisk * 100).toFixed(0)}%</p>
                <p><strong>Number of Stages:</strong> {currentPlan.stages.length}</p>
              </div>
            )}
          </div>

          <div className="resource-planning">
            <h2>🏺 Resource Requirements</h2>
            {currentPlan && (
              <ul>
                {currentPlan.resourceRequirements.map((resource, index) => (
                  <li key={index}>
                    {resource.amount} {resource.unit} of {resource.type}
                  </li>
                ))}
              </ul>
            )}
          </div>

          <div className="contingency-plans">
            <h2>⚡ Emergency Contingencies</h2>
            {currentPlan && (
              <ul>
                {currentPlan.contingencyPlans.map((plan, index) => (
                  <li key={index}>
                    <strong>{plan.trigger}:</strong> {plan.action}
                  </li>
                ))}
              </ul>
            )}
          </div>
        </div>

        {activeStage && currentPlan && (
          <StageDetailsPanel 
            stage={currentPlan.stages.find(s => s.id === activeStage)!} 
          />
        )}
      </div>

      <div className="journey-progress">
        <h2>🛤️ Journey Progress</h2>
        <div className="progress-bar">
          <div 
            className="progress-fill" 
            style={{ width: `${journeyProgress}%` }}
          />
        </div>
        <p>{journeyProgress.toFixed(1)}% Complete</p>
      </div>
    </div>
  );
};

// Helper interfaces and types
interface JourneyParameters {
  maxStageDistance: number;
  riskTolerance: number;
  availableTime: number;
  groupSize: number;
}

interface RiskAssessment {
  overallRisk: number;
  weatherRisk: number;
  floodRisk: number;
  navigationRisk: number;
  resourceRisk: number;
}

interface ContingencyPlan {
  trigger: string;
  action: string;
  resources: Resource[];
  alternativeRoutes: string[];
}

const calculateStageDuration = (distance: number, hazardLevel: number): number => {
  const baseTime = distance / 5; // 5 km per day base speed
  const hazardMultiplier = 1 + hazardLevel;
  return baseTime * hazardMultiplier;
};

const calculateTotalDistance = (stages: JourneyStage[]): number => {
  let total = 0;
  for (let i = 0; i < stages.length - 1; i++) {
    total += stages[i].position.distanceTo(stages[i + 1].position);
  }
  return total;
};

const calculateTotalDuration = (stages: JourneyStage[]): number => {
  return stages.reduce((total, stage) => total + stage.estimatedDuration, 0);
};

const calculateResourceRequirements = (stages: JourneyStage[]): Resource[] => {
  const resourceMap = new Map<string, Resource>();
  
  stages.forEach(stage => {
    stage.requiredResources.forEach(resource => {
      const key = resource.type;
      if (resourceMap.has(key)) {
        const existing = resourceMap.get(key)!;
        existing.amount += resource.amount;
      } else {
        resourceMap.set(key, { ...resource });
      }
    });
  });
  
  return Array.from(resourceMap.values());
};

const assessJourneyRisks = (stages: JourneyStage[]): RiskAssessment => {
  const overallRisk = stages.reduce((sum, stage) => sum + stage.hazardLevel, 0) / stages.length;
  
  return {
    overallRisk,
    weatherRisk: overallRisk * 0.3,
    floodRisk: overallRisk * 0.4,
    navigationRisk: overallRisk * 0.2,
    resourceRisk: overallRisk * 0.1
  };
};

const createContingencyPlans = (stages: JourneyStage[], risks: RiskAssessment): ContingencyPlan[] => {
  return [
    {
      trigger: "Severe weather conditions",
      action: "Seek immediate shelter and wait for conditions to improve",
      resources: [{ type: 'food', amount: 2, unit: 'days', priority: 'critical' }],
      alternativeRoutes: ["Coastal route", "Inland detour"]
    },
    {
      trigger: "Rising flood waters",
      action: "Move to higher ground and reassess route",
      resources: [{ type: 'water', amount: 5, unit: 'liters', priority: 'critical' }],
      alternativeRoutes: ["High ground path", "Bridge crossing"]
    },
    {
      trigger: "Navigation equipment failure",
      action: "Use stellar navigation and Homeric wayfinding techniques",
      resources: [{ type: 'tools', amount: 1, unit: 'set', priority: 'important' }],
      alternativeRoutes: ["Star-guided route", "Landmark navigation"]
    }
  ];
};

export default JourneyPlanningSystem;
```


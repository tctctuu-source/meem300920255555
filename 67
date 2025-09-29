import React from 'react';
import { Result, PosterStyles } from '../../../types';

interface PosterLayoutProps {
  program: { event: string; category: string };
  winners: Result[];
  styles: PosterStyles;
}

const StyledText: React.FC<{ style: any; children: React.ReactNode; className?: string }> = ({ style, children, className }) => {
  if (!style) return <div className={`text-red-500 ${className}`}>Style not defined</div>;
  
  const cssProperties: React.CSSProperties = {
    position: 'absolute',
    top: `${style.y}%`,
    left: `${style.x}%`,
    transform: 'translate(-50%, -50%)',
    width: '90%',
    fontSize: `${style.size}px`,
    color: style.color,
    textAlign: style.align,
    fontWeight: style.weight,
    fontFamily: `'${style.font}', sans-serif`,
    lineHeight: 1.2,
  };

  return <div style={cssProperties} className={className}>{children}</div>;
};

const PosterLayout4: React.FC<PosterLayoutProps> = ({ program, winners, styles }) => {
  const sortedWinners = [...winners].sort((a, b) => a.position - b.position);

  const getPlaceText = (position: number) => {
    if (position === 1) return 'First Place';
    if (position === 2) return 'Second Place';
    if (position === 3) return 'Third Place';
    return '';
  };

  return (
    <div className="w-full h-full bg-ui-surface text-ui-text-primary font-serif p-8 relative border-8 border-double border-brand-light-blue/20">
      <StyledText style={styles.headerText} className="tracking-[0.2em] uppercase">SSF DAAWA SECTOR</StyledText>
      <StyledText style={styles.mainTitle}>MUHIMMATH</StyledText>
      <div className="absolute top-[24%] left-1/2 -translate-x-1/2 w-24 h-px bg-brand-light-blue/50"></div>
      
      <StyledText style={styles.programName}>{program.event}</StyledText>
      <StyledText style={styles.category}>{program.category}</StyledText>
      
      {sortedWinners.map((winner, index) => (
        <React.Fragment key={winner.id}>
          <StyledText style={styles[`winner${index+1}Name`]}>{winner.participant}</StyledText>
          <StyledText style={styles[`winner${index+1}Team`]}>{getPlaceText(winner.position)}</StyledText>
        </React.Fragment>
      ))}
      
      <div className="absolute bottom-[14%] left-1/2 -translate-x-1/2 w-24 h-px bg-brand-light-blue/50"></div>
      <StyledText style={styles.footerText}>Results of 2025</StyledText>
    </div>
  );
};

export default PosterLayout4;
